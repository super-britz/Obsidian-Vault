# JuniApp-JuniGo架构

这张卡来自 [[33-跨境金融及求职讨论｜直播笔记整理]]，并根据用户提供的架构图重新校正。

[[JuniApp-JuniGo架构]] 是这套跨境金融系统的业务与技术架构名。图里写的是 `JuniApp / JuniGo 业务与技术架构图`，覆盖 C 端、后台、KYC/KYB、法币 Onramp/Offramp、链上钱包/转账、核心服务、数据与外部集成。

顶层技术栈：

- Monorepo：pnpm + Turbo。
- [[TypeScript]] 全栈。
- [[Hono API Gateway]]。
- [[PostgreSQL 与 Drizzle]]。
- [[Solana]]。
- [[Privy]]。
- [[Sumsub]]。
- AWS / [[Sentry]]。

分层：

- 入口与用户界面：C 端 Web、移动端 Native、运营后台、文档站点、外部回调入口。
- 业务能力层：账户认证、个人/企业、KYC/KYB、钱包与资产、链上转账、法币入金、法币出金。
- API 网关与核心服务：Hono API Gateway、路由层、核心业务服务、Admin 服务。
- 数据、队列与基础设施：PostgreSQL、Repository、Redis/BullMQ、AWS S3/SQS、Sentry/Pino、Cloudflare/AWS Lambda。
- 外部系统与通道：Privy、Solana Network、Sumsub、Tazapay、YellowCard、Busha、Mailgun/WhatsApp、Slack。

相关笔记：

- [[跨境金融系统]]
- [[Onramp]]
- [[Offramp]]
- [[双钱包归集]]
