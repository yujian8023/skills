# tushare-data fix round 1 summary

## Date
2026-03-23

## Scope
本总结记录 `tushare-data` 在第一轮真实试运行后，围绕 4 个核心问题所做的第一批修复，以及对应的修后复测结果。

## Current State
第一批 fix 已基本完成闭环：
- 关键问题已不是“skill 能不能跑”，而是“workflow 是否更贴题、更可信、更可交付”
- 修复后，财务趋势、筛选任务、导出任务、北向资金任务都完成了至少一轮针对性复测
- 当前可以判断：`tushare-data` 已经从“真实可用”进一步进入“开始稳定迭代”的阶段

## Problems Addressed
### 1. 财务趋势 workflow 不够贴题
#### Before
“最近几个季度营收和净利润趋势”类任务更容易偏向：
- ROE
- 毛利率
- 净利率

而不是营收与净利润本身。

#### Fix
- 在 `SKILL.md` 中明确：
  - `income` 作为营收 / 净利润趋势主数据源
  - `fina_indicator` 作为质量指标补充
- 在真实复测中，已改为用 `income` 主口径回答比亚迪最近 8 期营收和净利润变化

#### Result
修后结果明显更贴题，财务趋势类任务不再只像“财务质量快照”。

---

### 2. 北向资金汇总逻辑不稳定
#### Before
真实试运行中，`hsgt_top10` 的 `net_amount` 等字段出现不稳定或类型异常，导致直接做累计净买入汇总不可靠。

#### Fix
- 在 `SKILL.md` 中明确：
  - `moneyflow_hsgt` 用于整体北向方向判断
  - `hsgt_top10` 用于活跃成交明细
  - 当 `net_amount / buy / sell` 缺失或不稳定时，不强行做净买入排行榜
- 在单点修复中补了数值清洗逻辑：
  - 先对 `north_money` 等字段做数值标准化
  - 再判断净流入 / 净流出方向
  - 个股层只展示成交活跃名单

#### Result
修后已成功得到：
- 北向整体方向为净流入
- 最近 5 个交易日 `north_money` 可稳定读取
- 个股层可输出活跃成交前五名单

北向资金 workflow 已完成从“方向正确但实现不稳”到“方向和实现都更稳”的修复。

---

### 3. 筛选任务默认语义不足
#### Before
“高 ROE 低负债”筛选虽能跑通，但更像粗候选池，缺少研究语义约束。

#### Fix
- 在 `SKILL.md` 中明确区分：
  - 粗筛候选池
  - 研究级筛选
- 补充默认说明：
  - 可结合估值 / 市值 / 样本池说明
  - 若用户未给更细条件，应把当前结果定位为候选池而非最终结论
- 修后复测中，加入了 `PB < 15` 的估值约束

#### Result
筛选结果更接近研究筛选，而不是单纯机械条件过滤。

---

### 4. 导出任务说明层不完整
#### Before
导出任务真实可跑，但样本试跑和完整导出的边界不够清楚。

#### Fix
- 在 `SKILL.md` 中明确区分：
  - 样本试跑
  - 完整导出
  - dry-run 预演
- 输出时必须说明：
  - 当前是样本还是全量
  - 文件路径
  - 元信息

#### Result
修后复测中，导出任务已明确标注为“样本试跑”，避免样本结果被误读为完整导出。

## Files Updated In This Round
### Updated
- `/Users/yujian/.qclaw/workspace/skills/tushare-data/SKILL.md`

### Supporting reports written during this phase
- `/Users/yujian/.qclaw/workspace/skills/tushare-data/research/tushare-data-real-trial-report-2026-03-23.md`
- `/Users/yujian/.qclaw/workspace/skills/tushare-data/research/tushare-data-fix-plan-2026-03-23.md`

## Stable Rules After Round 1
以下规则在第一轮修复后应视为稳定：

- 财务趋势类任务：`income` 主、`fina_indicator` 辅
- 北向资金类任务：
  - `moneyflow_hsgt` 看方向
  - `hsgt_top10` 看活跃成交
  - 不强行依赖不稳定净买入字段做排行榜
- 筛选类任务：默认强调“候选池”语义，而不是过度包装成最终结论
- 导出类任务：必须说明是样本试跑、完整导出还是 dry-run

## What Is Now Considered Better Than Before
- 财务趋势回答更贴题
- 北向资金解释更稳
- 筛选结果更有研究语义
- 导出任务更不容易造成口径误读

## What Remains Incomplete
尽管第一批 fix 已闭环，但仍有后续可提升点：
- 财务趋势可继续补单季 / 累计 / 同比 / TTM 口径切换规则
- 筛选任务可继续补行业范围、市值范围、估值上限等默认参数
- 导出任务还需一次全量而非样本的完整闭环试运行
- 第三组边界 / 模糊 / 失败题还未全面进入真实复测阶段

## Recommended Next Steps
1. 进入第三组边界 / 失败 / 模糊真实题测试
2. 在边界题中重点验证：
   - 歧义标的
   - 模糊导出
   - 非交易日空表
   - 权限不足接口
   - 超短高频问法
3. 如第三组再暴露出问题，再按“试运行 → fix → retest”的节奏推进

## One-Line Summary
`tushare-data` 第一批 fix 已完成闭环，当前已经从“能跑”进一步升级到“主 workflow 更贴题、更稳、更可解释”的阶段。
