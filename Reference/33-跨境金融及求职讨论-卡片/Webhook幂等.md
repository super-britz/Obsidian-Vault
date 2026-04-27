# Webhook幂等

这张卡来自 [[33-跨境金融及求职讨论｜直播笔记整理]]。

[[Webhook幂等]] 是跨境金融系统里的关键工程点。第三方 provider、KYC 服务、支付通道都可能通过 webhook 通知状态变化。

问题是：

- webhook 可能重复发送。
- webhook 可能乱序到达。
- webhook 可能延迟很久。
- provider 可能临时故障。
- Lambda 调用是 stateless，不能依赖内存保存状态。

处理思路：

- 用业务单号和事件 ID 做幂等键。
- 把状态写入 [[PostgreSQL 与 Drizzle]] 或其他持久化存储。
- 用 [[SQS]] 解耦异步任务。
- 用 [[EventBridge]] 定时兜底和重试。
- 用 [[Sentry]] / [[Slack]] 做告警。

相关笔记：

- [[AWS基础部署链路]]
- [[跨境金融系统]]
- [[Payout Provider]]
