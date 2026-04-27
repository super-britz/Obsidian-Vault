# Privy

这张卡来自 [[33-跨境金融及求职讨论｜直播笔记整理]]，并根据架构图补充。

[[Privy]] 在架构图中承担账户、身份入口和钱包相关能力。

图中对应位置：

- C 端 Web：Privy React。
- 移动端 Native：Privy Expo。
- 账户与认证：Privy DID / Auth。
- 外部回调入口：`/webhooks/privy`。

在跨境金融系统中，Privy 不是 KYC 服务；KYC/KYB 由 [[Sumsub]] 承担。Privy 更接近登录、身份、钱包账户和用户 DID 的入口。

相关笔记：

- [[双钱包归集]]
- [[KYC]]
- [[JuniApp-JuniGo架构]]
