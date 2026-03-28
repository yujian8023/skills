# tushare-data implementation summary

## Date
2026-03-23

## Scope
本总结覆盖 `tushare-data` skill 从“接口目录型文档”重构为“任务路由型中文金融数据研究 skill”的这一轮完整改造。

## Current Positioning / State
`tushare-data` 现在已经不再是 Tushare 官方接口清单的搬运文档，而是一个面向中文自然语言请求的任务型 skill。

当前形态下，它可以围绕以下任务组织执行逻辑：
- 单标的行情查询
- 多标的横向对比
- 财务趋势与质量分析
- 估值与基本面快照
- 条件筛选与排行
- 资金流分析
- 板块 / 行业 / 主题轮动分析
- 公告 / 新闻 / 研报 / 政策梳理
- 宏观数据查询与简评
- 数据导出与研究准备
- 综合研究简报

它还补齐了 supporting references，使 skill 形成较完整的结构层次：
- `SKILL.md`：主 skill，负责定位、触发、任务分类、workflow、输出契约
- `references/endpoint-map.md`：任务到接口的辅助映射
- `references/examples.md`：中文自然语言 examples
- `references/workflow-playbooks.md`：工程执行 playbook
- `references/output-templates.md`：输出模板
- `references/faq.md`：常见异常与边界问题
- `references/regression-checklist.md`：回归测试清单

## What Changed
### 1. 主结构重写
原本的 `SKILL.md` 以平台介绍、安装说明、极简示例和超长接口列表为主，整体更像资料索引。

本轮已将其重写为：
- 面向任务而不是接口
- 面向中文自然语言而不是字段名
- 面向 workflow 而不是单点调用
- 面向输出交付而不是原始 DataFrame

### 2. 长接口表被降级为 reference
不再让主 skill 被长接口表主导，而是把“任务 → 可能接口”的职责剥离到 `references/endpoint-map.md`。

### 3. 增加自然语言层
新增 `examples.md`，覆盖常见中文请求，并进一步补充：
- 高频缩写（ROE / PB / PE / YoY / TTM）
- 数字化时间表达（8 个季度 / 20 个交易日 / 12 个月 / 近 5 年）
- 超短高频问法（北向最近买什么 / 今天最强板块 / 这票值不值得看）

### 4. 增加工程执行层
新增 `workflow-playbooks.md`，把真实执行中最关键的工程规则结构化，包括：
- preflight check
- 参数默认值
- 分段拉取策略
- 输出格式规范
- 元信息记录
- 数据质量检查
- 缓存与增量更新
- dry-run 指引

### 5. 增加输出模板层
新增 `output-templates.md`，统一了常见任务的输出长相，强调：
- 先结论
- 再口径与时间范围
- 再关键数字 / 表格
- 再限制说明
- 最后给文件路径（如有导出）

### 6. 增加 FAQ / pitfalls 层
新增 `faq.md`，把真实使用中高频出现的问题单独沉淀出来，包括：
- token 缺失
- tushare 包未安装
- 权限 / 积分不足
- 返回空表
- 名称歧义
- 日期格式问题
- 参数冲突
- 长区间请求过大
- 部分成功
- 实时口径 vs 盘后口径
- 财务口径混淆
- 板块口径混用
- 导出需求模糊

### 7. 增加回归测试基线
新增 `regression-checklist.md`，形成了一套可反复使用的验收清单，不再依赖临时人工拍脑袋测试。

## Stable Rules / Decisions
以下规则在本轮改造后应视为稳定：

- 主文档 `SKILL.md` 必须保持“任务路由型”，不能再次退化成接口百科
- 长接口表不应重新堆回主文档，接口映射应放在 `references/`
- 输出默认遵循“先结论，再证据，再口径，再限制”的顺序
- 中文自然语言优先，高频口语、缩写、短句都应被视为正常输入
- 模糊请求默认进入合理 workflow，不要一上来用接口名思维响应用户
- 异常处理必须显式区分：环境问题、权限问题、参数问题、空表但并非失败、口径差异
- 导出任务必须强调输出格式、文件路径、元信息与可复用性
- 真实长期可用性依赖 references 体系，而不是单个超长 SKILL.md

## Superseded Assumptions
以下旧假设已经不再成立：

- “只要把 Tushare 接口列全，这个 skill 就算完整了”
- “skill 主要职责是告诉代理有哪些 API”
- “官方文档型结构足以支持真实中文任务”
- “输出 DataFrame 或字段表就算完成数据技能”
- “主文档里塞进越多接口越好用”

这些旧思路已经被本轮重构明确替换。

## Current Governing Constraints
后续若继续维护 `tushare-data`，必须遵守这些当前约束：

- 以用户任务为中心，而不是以 endpoint 为中心
- 以中文自然语言触发为中心，而不是以专业术语触发为中心
- references 可以继续扩展，但主 skill 必须保持清晰、轻量、高信号
- 任何新增内容都应优先判断属于：主 workflow、reference、FAQ、examples、output template 中的哪一层
- 不要把“更多接口”误当成“更好 skill”；优先提高可执行性、可理解性和可交付性
- 每次重大结构调整后，应优先更新回归清单或 examples，而不是只补说明文字

## What Remains Incomplete
虽然这一轮已经形成完整骨架，但仍有几点可以继续提升：

- 可补一个 `references/script-patterns.md`，统一 Python 最小脚手架模式
- 可对 `regression-checklist.md` 做真实回归实测并记录得分
- 可根据真实使用反馈继续补高频表达、失败案例与输出模板边角场景
- 如后续要强调工程复用，可补数据缓存目录、脚本模板、metadata sidecar 的更细约定

## Recommended Next Steps
1. 用 `references/regression-checklist.md` 做一次真实回归测试，而不是停留在结构层自评
2. 根据回归结果补充 examples / FAQ 中未覆盖的真实高频问法
3. 如后续真实任务经常需要落 Python 脚本，再新增 `references/script-patterns.md`
4. 如果未来发生方向变化（例如从“研究 skill”转向“数据工程 skill”），必须额外补方向变更说明，避免旧理解复活

## One-Line Summary
`tushare-data` 已从“接口清单型文档”升级为“面向中文自然语言任务的金融数据研究 skill”，当前骨架完整，下一阶段重点应转向真实回归验证而非继续堆文档。
