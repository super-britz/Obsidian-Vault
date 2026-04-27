# Payout Provider

这张卡来自 [[33-跨境金融及求职讨论｜直播笔记整理]]。

[[Payout Provider]] 指跨境出金 / 法币结算服务商。它连接链上资产、银行账户、本地支付通道和目标法币。

在这场课里提到的 provider 包括：

- [[Tazapay]]
- [[Yellow Card|YellowCard]]
- [[Busha]]

结合架构图，provider 能力应拆成：

- [[Onramp]]：法币入金，Direct Deposit，quote / pay / arrive。
- [[Offramp]]：法币出金，Beneficiary，quote / payout / status。

典型职责：

- 接收平台转来的稳定币或结算指令。
- 根据国家、汇率和费用选择出金通道。
- 创建或校验收款账户。
- 把资金结算为美元、人民币或其他目标国家法币。

相关笔记：

- [[跨境金融系统]]
- [[法币出入金]]
- [[同名账户转账]]
