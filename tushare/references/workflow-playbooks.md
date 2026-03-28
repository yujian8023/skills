# Workflow Playbooks

This file contains operational playbooks for `tushare-data`.
Use it to keep execution stable, outputs reusable, and user-facing results clear.

---

## 1. Default execution pattern

For most requests, follow this order:
1. Understand the task
2. Resolve entities (stock / index / ETF / market)
3. Normalize parameters (dates, codes, ranges)
4. Choose the smallest useful dataset
5. Fetch in safe batches if needed
6. Validate and clean the result
7. Deliver a human-readable conclusion first
8. Export files when useful

Do not start from endpoint names.
Start from user intent.

---

## 2. Preflight checklist

Before calling Tushare:
- confirm Python is available
- confirm `tushare` is installed
- confirm `TUSHARE_TOKEN` exists
- if task is large or premium, warn about possible permission / score limits
- if this is the first call in a workflow, consider a light smoke test

If token is missing, stop early and explain how to configure it.

---

## 3. Parameter defaults

### Time defaults
- recent price action → 20 trading days
- recent trend / "最近一段时间" → 3 months
- financials / earnings → latest 8 quarters + latest annual period
- fund flow trend → 5 to 20 trading days depending on question
- macro context → latest 6 to 12 observations

### Code defaults
- normalize to `600519.SH` / `000001.SZ` style
- do not blindly guess when ambiguity is high

### Category defaults
- industry → prefer申万/中信等较稳定口径
- concept/theme → prefer同花顺/东财类主题口径
- if classification matters, state the chosen taxonomy

---

## 4. Chunking rules

Use chunking by default for larger requests.

### Suggested chunking
- daily / weekly / monthly data over long spans → split by year or quarter
- minute data → split by month or week
- financial data → split by year / report period
- multi-asset tasks → batch by asset and then by date range

### Chunking discipline
- retry failed chunks only
- merge after fetch
- de-duplicate on primary keys
- sort consistently
- keep track of failed chunks for reporting

---

## 5. Output format rules

### Small result
Use:
- short conclusion
- key metrics
- a small markdown table if helpful

### Medium result
Use:
- short conclusion
- summary table
- CSV export if the data may be reused

### Large result / research dataset
Use:
- short conclusion
- data scope and schema summary
- parquet preferred
- optional CSV when interoperability matters

### Export naming
Include:
- endpoint or task type
- target or scope
- date range
- timestamp

Examples:
- `daily_600519.SH_20230101_20231231_20260322.csv`
- `factor_screen_semiconductor_20260322.parquet`

---

## 6. Metadata sidecar rule

When exporting files, also provide short metadata in the user-visible summary or a sidecar note:
- endpoint(s) used
- parameters
- fetch time
- row count
- fields
- failed chunks if any
- whether cache was used

This makes downstream reuse much safer.

---

## 7. Human-readable response contract

Prefer this response order:
1. One-line conclusion
2. Scope and time range
3. Key numbers or top findings
4. Caveats / limits / missing data
5. Output path(s) if files were saved

Do not dump raw field names without interpretation unless the user explicitly asks.

---

## 8. Data quality checks

After fetching data, check:
- expected columns exist
- no obviously broken schema
- primary-key duplicates removed
- dates normalized
- numeric fields cast when appropriate
- final sort order is stable

### Empty results
Differentiate between:
- non-trading day
- no data in range
- asset not yet listed
- parameter mistake
- permission issue

Do not report all empty tables as generic failure.

---

## 9. Error handling

### Retry only for transient errors
Retry for:
- timeout
- temporary network failure
- rate-limit / 429 style failures

Do not blindly retry:
- missing token
- permission / score issues
- invalid parameters
- unsupported fields

### Partial success
If only some chunks succeed:
- say it is partially complete
- name the failed ranges or assets
- report whether an incomplete file was still saved

---

## 10. Cache and incremental update

Prefer reusable behavior:
- cache static or slow-changing tables like `stock_basic`, trading calendars, index basics
- prefer incremental update over full re-download
- when possible, resume from completed chunks
- make cache usage visible in the summary

---

## 11. Task-specific mini playbooks

### A. Single-stock quick read
Use when user asks for a fast view of one stock.
Output:
- one-line verdict
- recent price action
- turnover/volume note
- optional extension to valuation or financials

### B. Financial trend read
Use when user asks how earnings or quality changed.

Priority rule:
- if user explicitly asks for revenue / net profit trend, use `income` first
- then use `fina_indicator` only as a quality supplement
- do not let ROE / margin metrics replace the main requested trend

Output:
- trend summary
- 4–8 quarter key table
- note on yoy / single-quarter / cumulative basis
- separate main metrics from quality metrics

### C. Comparison
Use when user asks who is stronger / cheaper / steadier.
Output:
- comparison table
- best/worst by each metric
- short synthesis paragraph

### D. Screening
Use when user asks to find assets by conditions.
Output:
- universe definition
- filter rules
- ranking rule
- top results
- caveat on missing / stale data if relevant

### E. Event explanation
Use when user asks why a stock or sector moved.
Output:
- likely factors, not false certainty
- combine price action + announcements + news + flows
- clearly separate facts from interpretation

### F. Export job
Use when user asks for files.
Output:
- what was exported
- date range / asset scope
- row counts
- file paths
- whether data was chunked or partial

---

## 12. Dry-run guidance

For large jobs, it is good practice to first say:
- which dataset(s) will be pulled
- date range / symbol scope
- output format
- chunking approach
- likely runtime / risk points

Then execute.

---

## 13. Quick rule

A good `tushare-data` execution should feel like:
- a research assistant understood the question
- a data engineer fetched the right tables safely
- an analyst summarized the result clearly

not like someone pasted raw API output.
stood the question
- a data engineer fetched the right tables safely
- an analyst summarized the result clearly

not like someone pasted raw API output.
