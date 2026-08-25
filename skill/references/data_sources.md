# 数据源、计算口径与缺失处理

## 来源优先级

| 数据类别 | 主来源 | 交叉核验/后备 | 使用要求 |
|---|---|---|---|
| 全球权益、外汇、商品、加密资产 | 有许可的市场数据服务或交易所/指数管理方公开页面 | 可追溯公开日线服务 | 记录代码、时区、正式收盘日和数据许可。 |
| VIX、VXN | Cboe 指数页或许可数据源 | FRED 的 VIXCLS、VXNCLS | 使用同一有效交易日；核验日期与数值。 |
| 美国 10 年期与 2 年期收益率 | FRED：DGS10、DGS2 | 美国财政部或同等权威序列 | 单位为百分比；变化使用基点。 |
| 宏观与政策 | 美联储、ECB、PBOC、BOJ 等官方发布 | 权威媒体仅作背景 | 政策事实优先官方。 |

> 使用任何 API 或网页前，需确认其服务条款、许可范围、速率限制、再分发限制和署名要求。本项目不附带数据订阅、密钥、网页抓取代码或任何授权保证。

## 覆盖范围示例

| 类别 | 可覆盖项目 |
|---|---|
| 股票 | S&P 500、Nasdaq Composite、Dow Jones、Euro Stoxx 50、FTSE 100、DAX、Nikkei 225、Hang Seng、CSI 300。 |
| 债券 | 美国 10 年期收益率、美国 2 年期收益率、2s10s 利差。 |
| 外汇 | DXY、USD/CNY、USD/JPY、EUR/USD。 |
| 商品与加密资产 | 黄金、布伦特、WTI、铜、Bitcoin、Ethereum。 |
| 风险 | VIX、VXN、VXN-VIX、VXN/VIX。 |

## 计算定义

设最新有效观测为 \(P_t\)，前一有效观测为 \(P_{t-1}\)，第 5 个前序有效观测为 \(P_{t-5}\)。

| 指标 | 公式 | 注意事项 |
|---|---|---|
| 日变动 | \((P_t/P_{t-1}-1)×100\%\) | 使用相邻有效交易日。 |
| 近 5 日变动 | \((P_t/P_{t-5}-1)×100\%\) | 使用 5 个有效交易日/观测日。 |
| VXN-VIX | \(VXN_t-VIX_t\) | 仅在日期同步时计算。 |
| VXN/VIX | \(VXN_t/VIX_t\) | 仅在日期同步时计算。 |
| 收益率日变动 | \((Y_t-Y_{t-1})×100\) bp | 不使用收益率百分比涨跌替代基点变化。 |
| 2s10s | \((DGS10_t-DGS2_t)×100\) bp | DGS10 与 DGS2 必须使用同一日期。 |

## 缺失与异常规则

1. 有效观测少于计算窗口所需数量时，标记缺失，不外推。
2. VIX 与 VXN 日期不同步时，停止计算最新价差、比值和成长股风险扩散结论。
3. DGS2 缺失时，2s10s 同时标记缺失；不得用 13 周国库券、利率期货或任何非等价短端代理替代。
4. 市场休市、时区差异或数据延迟导致交易日不一致时，保留各自最近正式收盘日并在数据状态栏说明。
5. 来源产生未解释的重大冲突时，优先回溯至原始/权威来源；在冲突解决前不写入确定性结论。

## 官方与可追溯来源

- [Cboe VIX Historical Data](https://www.cboe.com/en/tradable-products/vix/vix-historical-data/)
- [Cboe VXN Index Dashboard](https://www.cboe.com/us/indices/dashboard/vxn/)
- [FRED VIXCLS](https://fred.stlouisfed.org/series/VIXCLS)
- [FRED VXNCLS](https://fred.stlouisfed.org/series/VXNCLS)
- [FRED DGS10](https://fred.stlouisfed.org/series/DGS10)
- [FRED DGS2](https://fred.stlouisfed.org/series/DGS2)
- [Federal Reserve](https://www.federalreserve.gov/)
- [European Central Bank](https://www.ecb.europa.eu/)
- [People's Bank of China](http://www.pbc.gov.cn/)
- [Bank of Japan](https://www.boj.or.jp/en/)
