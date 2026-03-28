# tushare-data fix plan

## Date
2026-03-23

## Scope
本计划基于第一轮真实试运行后暴露出的 4 个核心问题，给出当前最优先的修正方向与落地建议。

## Problem 1 — 财务趋势 workflow 不够贴题
### Observed issue
在“看下比亚迪最近8个季度营收和净利润趋势”这类请求中，当前 workflow 更容易偏向 `fina_indicator`，导致输出更像：
- ROE
- 毛利率
- 净利率

而不是用户真正点名要看的：
- 营收
- 净利润

### Verified finding
真实探测中，`income` 接口可用，且直接提供：
- `total_revenue`
- `revenue`
- `n_income`

这比单纯依赖 `fina_indicator` 更适合作为“营收 / 净利润趋势”类任务的主数据源。

### Fix direction
- 对“营收趋势 / 净利润趋势 / 最近几个季度业绩”类请求，默认优先：`income`
- 再用 `fina_indicator` 补充：ROE、毛利率、净利率等质量指标
- 输出时明确区分：
  - 主指标（营收 / 净利润）
  - 补充指标（ROE / 毛利率 / 净利率）

### Priority
High

---

## Problem 2 — 北向资金汇总逻辑需校验
### Observed issue
“北向资金最近在买什么”真实试运行中，链路虽跑通，但汇总后的 `net_amount` 出现异常解释风险。

### Verified finding
当前 `hsgt_top10` 接口真实字段中，包含：
- `amount`
- `net_amount`
- `buy`
- `sell`

但部分样本中 `net_amount / buy / sell` 展示为空字符串，说明：
- 该接口字段在当前口径下并不总是稳定可用
- 或在现有聚合逻辑下，不宜直接把 `net_amount` 当成可靠累计净买入口径

### Fix direction
- 对北向资金任务，优先把 `hsgt_top10` 视为“活跃成交与上榜明细”来源，而不是直接稳定的累计净买入口径
- 如 `net_amount` 缺失或不稳定，改为：
  - 先输出高成交活跃名单
  - 再明确说明净买入口径受限
- 若需要更稳的方向判断，优先配合 `moneyflow_hsgt` 做宏观北向流向判断，而不是强行在个股层做不稳聚合

### Priority
High

---

## Problem 3 — 筛选任务默认约束不足
### Observed issue
“高 ROE 低负债”筛选真实可跑，但结果更像粗候选池，而非研究级筛选结果。

### Verified finding
真实样本里已经能同时得到：
- `roe`
- `pb`
- `pe`
- `total_mv`
- `debt_ratio`

说明当前数据层已经足以支持更合理的默认约束。

### Fix direction
筛选任务默认应区分：
- **基础筛选条件**：如 ROE、负债率
- **质量修饰条件**：如 PE / PB 不过高、总市值范围、行业/指数样本池

推荐默认行为：
- 输出时明确说明这是“候选池”
- 若用户没指定更细条件，可建议：
  - 增加估值约束
  - 增加行业 / 指数范围
  - 增加市值门槛
- 输出模板中增加：
  - 样本池说明
  - 条件说明
  - 结果并非最终结论的提醒

### Priority
Medium-High

---

## Problem 4 — 导出任务尚未完整闭环
### Observed issue
真实导出已成功，但当前是样本试跑（沪深300前 10 只股票），还不是完整业务闭环。

### Verified finding
- CSV 导出链路已可用
- 输出文件路径可生成
- 数据行数与标的数可追踪

说明导出 workflow 是真的可跑的，只差规模化与说明层完善。

### Fix direction
对导出类任务增加明确区分：
- 样本试跑
- 完整导出
- dry-run 预演

完整导出时，默认补充：
- 分段拉取策略
- 文件规模预估
- 标的范围说明
- 是否全量 or 样本
- 元信息 sidecar

### Priority
Medium

---

## Stable Decisions
- 现在的核心问题已不是“skill 能不能跑”，而是“workflow 是否足够贴题和可信”
- 修复应优先发生在 workflow 设计层，而不是重新堆文档
- 后续每修一个问题，都应优先回到真实任务里复测，而不是只更新说明文件

## Recommended Next Steps
1. 调整财务趋势类 workflow：把 `income` 提升为主数据源
2. 调整北向资金解释策略：从“净买入汇总”转向“活跃明细 + 方向说明”
3. 调整筛选类 workflow：默认增加候选池语义、估值/样本池说明
4. 为导出类任务增加“样本试跑 / 完整导出 / dry-run”三层说明
5. 上述 4 项修正后，再回到真实业务题复测对应 case

## One-Line Summary
`tushare-data` 当前已通过真实试运行，后续优化重点不是继续补文档，而是修正具体 workflow 的贴题度、可信度与可交付性。
