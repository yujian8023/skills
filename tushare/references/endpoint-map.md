# Endpoint Map

This file is a supporting reference for `tushare-data`.
Use it as a lookup sheet, not as the main skill flow.

The main skill should stay task-oriented.
This file exists to map common task categories to likely Tushare endpoints.

---

## 1. 行情 / 趋势

### 常用接口
- `daily` — 股票历史日线
- `pro_bar` — 复权行情 / 通用行情
- `weekly` — 周线
- `monthly` — 月线
- `stk_mins` — 历史分钟
- `rt_k` — 实时日线
- `rt_min` — 实时分钟
- `adj_factor` — 复权因子
- `daily_basic` — 每日基本指标

### 常见任务
- 某股票近一段时间走势
- 区间涨跌幅、成交量、换手率
- 高低点、回撤、波动
- 复权行情导出

---

## 2. 股票 / 指数 / 基金基础资料

### 常用接口
- `stock_basic` — 股票列表
- `stock_company` — 上市公司基础信息
- `fund_basic` — 公募基金列表
- `index_basic` — 指数基础信息
- `new_share` — IPO 新股
- `stock_st` / `st` — ST 列表
- `namechange` — 曾用名

### 常见任务
- 标的识别
- 股票名转代码
- 公司基本信息
- 指数 / ETF / 基金基础资料

---

## 3. 财务 / 公司质量

### 常用接口
- `fina_indicator` — 财务指标
- `income` — 利润表
- `balancesheet` — 资产负债表
- `cashflow` — 现金流量表
- `forecast` — 业绩预告
- `express` — 业绩快报
- `disclosure_date` — 财报披露计划
- `fina_audit` — 财务审计意见
- `dividend` — 分红送股

### 常见任务
- 营收 / 利润趋势
- ROE / 毛利率 / 现金流质量
- 资产负债结构
- 财报节奏 / 财务事件

---

## 4. 估值 / 每日基本面

### 常用接口
- `daily_basic` — PE / PB / PS / 市值 / 股息率等
- `fina_indicator` — 补充质量型指标

### 常见任务
- 当前估值高不高
- 多标的估值对比
- 低估值高分红筛选

---

## 5. 资金流 / 交易行为

### 常用接口
- `moneyflow` — 个股资金流向
- `moneyflow_hsgt` — 沪深港通资金流向
- `hsgt_top10` — 沪深股通十大成交股
- `ggt_top10` — 港股通十大成交股
- `top_list` — 龙虎榜每日统计单
- `top_inst` — 龙虎榜机构交易单
- `moneyflow_ind_ths` — THS 行业资金流
- `moneyflow_ind_dc` — DC 板块资金流
- `moneyflow_mkt_dc` — DC 大盘资金流

### 常见任务
- 北向最近买什么
- 主力资金流入最多的是谁
- 龙虎榜活跃标的
- 板块资金流向

---

## 6. 板块 / 行业 / 主题

### 常用接口
- `index_classify` — 申万行业分类
- `index_member_all` — 申万行业成分
- `sw_daily` — 申万行业指数日行情
- `ths_index` — 同花顺概念/行业板块
- `ths_member` — 同花顺板块成分
- `ths_daily` — 同花顺板块行情
- `dc_index` — 东方财富概念板块
- `dc_member` — 东方财富板块成分
- `dc_daily` — 东方财富概念/行业行情
- `ci_daily` — 中信行业指数日行情

### 常见任务
- 最近哪个板块最强
- 板块轮动
- 概念板块成分股
- 行业排行

---

## 7. 打板 / 情绪 / 活跃度

### 常用接口
- `limit_list_d` — 涨跌停和炸板数据
- `limit_step` — 连板天梯
- `limit_cpt_list` — 涨停最强板块统计
- `kpl_list` — 开盘啦榜单
- `dc_hot` — 东方财富热榜
- `ths_hot` — 同花顺热榜
- `hm_list` / `hm_detail` — 游资名录与明细

### 常见任务
- 今日市场情绪
- 连板梯队
- 炸板率 / 热门题材
- 游资活跃方向

---

## 8. 公告 / 新闻 / 研报 / 政策

### 常用接口
- `anns_d` — 上市公司公告
- `news` — 新闻快讯
- `major_news` — 长篇通讯
- `research_report` — 券商研究报告
- `npr` — 国家政策库
- `irm_qa_sh` — 上证 e 互动问答
- `irm_qa_sz` — 深证易互动问答
- `cctv_news` — 新闻联播文字稿

### 常见任务
- 最近公告梳理
- 是否存在催化事件
- 行业 / 板块新闻面
- 研报检索
- 政策影响分析

---

## 9. 宏观数据

### 常用接口
- `cn_cpi` — CPI
- `cn_ppi` — PPI
- `cn_pmi` — PMI
- `cn_gdp` — GDP
- `cn_m` — 货币供应量
- `sf_month` — 社融
- `shibor` — Shibor
- `shibor_lpr` — LPR
- `us_tycr` — 美国国债收益率曲线
- `index_global` — 国际主要指数

### 常见任务
- 宏观环境变化
- 流动性与利率
- 风格环境判断
- 跨市场背景补充

---

## 10. 导出 / 数据准备

### 说明
导出并不是单独接口，而是由上游任务决定取哪些数据，再统一做：
- 分段拉取
- 去重
- 标准化
- 落盘
- 写元信息

### 推荐输出
- 小表：Markdown + CSV
- 中表：CSV
- 大表 / 回测 / 建模：Parquet

---

## Recommended core set

若只记一套最常用接口，优先这批：
- `stock_basic`
- `trade_cal`
- `daily`
- `pro_bar`
- `daily_basic`
- `fina_indicator`
- `income`
- `balancesheet`
- `cashflow`
- `forecast`
- `express`
- `moneyflow`
- `moneyflow_hsgt`
- `hsgt_top10`
- `top_list`
- `index_basic`
- `index_daily`
- `index_classify`
- `sw_daily`
- `ths_index`
- `ths_member`
- `limit_list_d`
- `limit_step`
- `news`
- `research_report`
- `anns_d`
- `cn_cpi`
- `cn_pmi`
- `us_tycr`

---

## Quick rule

主 skill 负责回答：
- 用户在问什么任务
- 该怎么做
- 结果该如何交付

本文件只负责回答：
- 这个任务大概率会用到哪些接口
