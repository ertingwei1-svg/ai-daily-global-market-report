# 全球市场策略日报

一个面向研究与监控场景的**中文全球市场日报框架**。项目提供可复用的技能说明、数据口径、报告模板及质量控制规则，用于生成包含 VIX、VXN、成长股风险扩散、跨资产回顾、风险仪表盘、一般性资产配置框架和次日观察清单的日报。

> **定位：** 本项目是研究与报告自动化的公开模板，不包含任何个人邮箱、账户、连接器标识、计划任务标识、访问令牌、历史运行日志或原始客户附件。

## 内容概览

| 路径 | 内容 |
|---|---|
| `skill/SKILL.md` | 报告生成工作流与成长股风险扩散规则 |
| `skill/references/data_sources.md` | 数据源优先级、符号映射、计算口径与缺失处理 |
| `skill/templates/daily_report_template.md` | 可复用的中文 Markdown 报告模板 |

## 使用方式

首先阅读 `skill/references/data_sources.md`，并将实际可用的公开或授权数据源映射到模板中的资产项。然后按 `skill/SKILL.md` 的工作流收集数据，使用 `skill/templates/daily_report_template.md` 填充报告。对每一项市场数据记录数据日期与来源；对缺失项记录原因和结论限制。

若部署为定时任务，应将时区、触发时间、收件人和投递渠道放在部署环境的安全配置中，而不是写入本仓库。发送前应确认收件人与投递授权，并通过幂等键或数据截点避免重复发送。

## 方法要点

日变动定义为最新有效观测相对前一有效观测的百分比，近 5 日变动定义为最新有效观测相对第 5 个前序有效观测的百分比。美国收益率变化使用百分点差换算为基点；2s10s 使用同日 DGS10 减 DGS2 计算。VIX/VXN 仅在日期同步时计算最新相对风险状态；日期不同步时必须停止对成长股风险扩散作结论。

> **成长股风险扩散判定：** 当 VXN 上升、VIX 横盘或下降，且 VXN-VIX 价差扩大时，视为相对扩散确认；当两者同跌而价差收窄时，表述为“风险溢价仍高，但暂未进一步扩散”。任何一项数据缺失或日期不同步时，结论为“无法确认”。

## 数据来源与可追溯性

Cboe 提供 VIX 及 VXN 的指数和历史数据入口；FRED 提供 VIXCLS、VXNCLS、DGS10 与 DGS2 等可引用的时序。中央银行与宏观事实优先使用官方发布页面。[1] [2] [3] [4] [5]

使用任何数据提供方前，应遵守其服务条款、再分发限制及署名要求。开放的行情页面不当然授予批量采集、再发布或商业使用权。

## 质量检查

| 检查项 | 通过标准 |
|---|---|
| 数据日期 | 每个资产都有数据日期；不同市场的收盘日差异已披露。 |
| 可复算性 | 日变动、近 5 日变动、价差、比值和利差均能从列示输入值复算。 |
| 缺失处理 | 不以替代序列冒充正式序列；不估算缺失行情或政策事实。 |
| 事实/判断 | 所有事实有来源；所有判断有依据、反证条件或不确定性说明。 |
| 投资合规 | 仅输出一般性研究观点，不对具体个人或账户给出买卖建议。 |
| 发布安全 | 不提交邮箱地址、账户名、API Key、令牌、连接器 ID、历史运行日志或客户文件。 |

## 免责声明

本仓库仅提供研究、教育和自动化模板，**不构成个性化投资建议、证券研究报告或交易指令**。使用者应独立核验数据与方法，并对其部署、数据许可、信息安全和投资决策负责。

## 许可

本项目采用 [MIT License](LICENSE)。

## References

[1]: https://www.cboe.com/en/tradable-products/vix/vix-historical-data/ "Cboe VIX Historical Data"
[2]: https://www.cboe.com/us/indices/dashboard/vxn/ "Cboe VXN Index Dashboard"
[3]: https://fred.stlouisfed.org/series/VIXCLS "FRED: CBOE Volatility Index: VIX"
[4]: https://fred.stlouisfed.org/series/VXNCLS "FRED: CBOE NASDAQ 100 Volatility Index"
[5]: https://fred.stlouisfed.org/series/DGS10 "FRED: 10-Year Treasury Constant Maturity Rate"
