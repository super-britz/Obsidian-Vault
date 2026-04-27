# Offramp

这张卡来自 [[33-跨境金融及求职讨论｜直播笔记整理]]，并根据架构图补充。

[[Offramp]] 指法币出金，也就是把平台内资产或链上稳定币转换成目标国家/地区的法币并打到收款人账户。

架构图里的对应模块是：

- 法币出金。
- `Offramp / Beneficiary`。
- `Quote / Payout / Status`。

典型流程：

- 创建出金 quote。
- 选择 beneficiary。
- 提交 payout。
- 等待 provider 状态回调。
- 更新订单状态并通知用户。

面试重点：

- 出金状态机如何设计。
- provider 回调如何做幂等。
- 失败、延迟、退回怎么处理。
- KYC/KYB/KYT/AML 如何影响出金权限。

相关笔记：

- [[Payout Provider]]
- [[法币出入金]]
- [[Webhook幂等]]
