# Regression Checklist

Use this checklist to validate whether `tushare-data` is truly usable in real conversations.

The goal is not just to see whether the skill *looks complete*.
The goal is to verify whether it:
- triggers on natural user language
- routes intent correctly
- follows a sensible workflow
- outputs useful human-readable results
- handles ambiguity and failure without getting messy

---

## How to score

For each case, score these dimensions:

### 1. Trigger hit
- 0 = wrong skill / no trigger
- 1 = partial / uncertain trigger
- 2 = clear and natural trigger

### 2. Intent routing
- 0 = wrong task category
- 1 = roughly correct but unstable
- 2 = clearly correct

### 3. Workflow quality
- 0 = no clear workflow
- 1 = workflow exists but weak
- 2 = workflow is sensible and task-appropriate

### 4. Output quality
- 0 = raw field dump / not useful
- 1 = somewhat useful but rough
- 2 = clear conclusion + useful evidence + scope/caveats

### 5. Failure / ambiguity handling
- 0 = messy or misleading
- 1 = partially handled
- 2 = handled clearly and safely

### Suggested rating bands
- **85+** → mature enough for long-term use
- **70–84** → usable with small fixes
- **55–69** → correct direction but still unstable in practice
- **<55** → still more like documentation than a real skill

---

## A. High-frequency normal requests

### Case 1 — Single-stock price action
**Request:** `看下宁德时代最近三个月走势`

**Expected:**
- trigger行情 / 趋势
- route到单标的行情分析
- output区间涨跌、高低点、活跃度、简短判断

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 2 — Research brief
**Request:** `给我快速研究一下中际旭创`

**Expected:**
- trigger综合研究简报
- automatically combine行情、财务、估值、资金、公告
- output像简报，不像字段堆砌

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 3 — Financial trend
**Request:** `看下比亚迪最近8个季度营收和净利润趋势`

**Expected:**
- trigger财务 / 公司质量
- route到财务质量快照
- identify“8个季度”
- mention口径（同比 / 单季 / 累计）

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 4 — Valuation snapshot
**Request:** `茅台现在估值算高吗`

**Expected:**
- trigger估值 / 基本面
- output PE/PB/股息率等关键指标
- explain valuation scope, not just say “高/低”

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 5 — Multi-asset comparison
**Request:** `比一下茅台、五粮液、泸州老窖近一年的涨幅和估值`

**Expected:**
- trigger多标的横向对比
- consistent time window
- comparison table + synthesis

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 6 — Screening
**Request:** `帮我筛一批高ROE低负债的股票`

**Expected:**
- trigger筛选 / 财务筛选
- understand ROE
- define universe + filter logic + ranking rule

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 7 — Fund flow
**Request:** `北向资金最近在买什么`

**Expected:**
- trigger资金流追踪
- explain口径与时间窗
- show sustained flow instead of one-day noise

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 8 — Sector rotation
**Request:** `最近哪个板块最强`

**Expected:**
- trigger板块轮动
- classify by a stated taxonomy
- output ranking + representatives

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

## B. Colloquial / shorthand / short-form requests

### Case 9 — Very short price-action request
**Request:** `这票最近强不强`

**Expected:**
- still trigger行情快照
- not collapse into confusion because the wording is short

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 10 — Shorthand metrics
**Request:** `帮我找PB低、ROE高、YoY改善明显的票`

**Expected:**
- recognize PB / ROE / YoY
- route to screening + financial/valuation analysis

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 11 — Time expression
**Request:** `看下最近20个交易日谁最抗跌`

**Expected:**
- understand 20 trading days as a real range
- route to ranking/screening instead of generic行情

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 12 — Ultra-short board/sector question
**Request:** `今天最强板块`

**Expected:**
- route to板块/情绪
- infer short time horizon from “今天”

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 13 — Colloquial research intent
**Request:** `这票值不值得看`

**Expected:**
- route to快速研究 / 简报模式
- output summary, not raw tables

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 14 — Catalyst short-form question
**Request:** `最近有没有催化`

**Expected:**
- route to公告 / 新闻 / 事件梳理
- if object missing, ask minimal clarification instead of guessing wildly

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

## C. Edge / failure / ambiguity cases

### Case 15 — Ambiguous name
**Request:** `帮我看下中航最近怎么样`

**Expected:**
- do not blindly guess
- show candidates / ask minimal clarification

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 16 — Vague export request
**Request:** `给我导一份数据`

**Expected:**
- route to导出 / 研究准备
- minimally clarify scope / symbols / time / format

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 17 — Empty-result risk
**Request:** `帮我查一个非交易日的日线数据`

**Expected:**
- recognize non-trading-day possibility
- explain empty result carefully instead of generic failure

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

### Case 18 — Missing token
**Request:** any real data task in an environment without `TUSHARE_TOKEN`

**Expected:**
- fail early during environment check
- explain shortest fix path
- do not pretend it can still fetch data

| Dimension | Score | Notes |
|-----------|-------|-------|
| Trigger hit |   |   |
| Intent routing |   |   |
| Workflow quality |   |   |
| Output quality |   |   |
| Failure/ambiguity handling |   |   |

---

## Summary sheet

Use this table after running all cases:

| Group | Cases | Max Score | Actual Score | Notes |
|-------|-------|-----------|--------------|-------|
| A. Normal requests | 8 | 80 |  |  |
| B. Colloquial / shorthand | 6 | 60 |  |  |
| C. Edge / failure cases | 4 | 40 |  |  |
| **Total** | **18** | **180** |  |  |

### Normalized interpretation
- **153+ / 180** → 85+，成熟可长期使用
- **126–152 / 180** → 70–84，可用但还可精修
- **99–125 / 180** → 55–69，方向对但还不稳
- **<99 / 180** → 还偏文档化，不够实战

---

## Quick rule

A passing `tushare-data` skill should feel like:
- it understands what the user means
- it chooses a reasonable workflow
- it explains results like an analyst
- it behaves carefully like a data engineer

not like a pasted API manual.
