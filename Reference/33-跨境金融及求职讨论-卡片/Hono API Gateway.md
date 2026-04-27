# Hono API Gateway

这张卡来自 [[33-跨境金融及求职讨论｜直播笔记整理]]，并根据架构图补充。

[[Hono API Gateway]] 是这套系统的统一 API 入口。它不是单纯写几个 Hono 路由，而是承担网关职责。

架构图里的职责：

- CORS。
- CSRF。
- RateLimit。
- Error 统一处理。
- Sentry init。
- Env 校验。
- Lambda / HTTP 入口适配。

它向后连接 [[路由层]]，再进入 [[核心业务服务]]。

相关笔记：

- [[Hono]]
- [[Webhook幂等]]
- [[Cloudflare 与 AWS Lambda]]
