# Cloudflare 与 AWS Lambda

这张卡来自 [[33-跨境金融及求职讨论｜直播笔记整理]]，并根据架构图补充。

[[Cloudflare 与 AWS Lambda]] 是架构图里基础设施层的一组部署能力。它修正了转写里的“Cloudera”。

可理解为：

- Cloudflare：边缘入口、DNS、缓存、安全防护、可能承接部分边缘函数。
- AWS Lambda：Serverless 后端计算，承载 API、webhook、异步任务入口。

和跨境金融系统相关的重点：

- Lambda 是 stateless，不能依赖内存保存长流程状态。
- 外部 provider 回调要落库、幂等、可重试。
- Cloudflare 可以做入口层防护，但业务状态必须在服务端持久化。

相关笔记：

- [[Hono API Gateway]]
- [[Webhook幂等]]
- [[AWS基础部署链路]]
