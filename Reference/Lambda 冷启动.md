---
tags:
  - AWS
  - Lambda
  - Serverless
  - 性能
aliases:
  - 冷启动
  - Lambda冷启动
status: 已整理
---

# Lambda 冷启动

[[Lambda 冷启动]] 指函数长时间没有请求后，平台需要重新准备运行环境、加载代码、初始化依赖，然后才能执行 handler。

它通常受这些因素影响：

- 代码包体积。
- 运行时类型。
- 依赖初始化成本。
- 是否需要连接数据库、加载配置、初始化 SDK。
- 是否在 VPC 内访问资源。
- 是否使用 Layers 或自定义镜像。

在 SSR 场景下，冷启动会直接影响 [[TTFB 与 Web Vitals]]。如果用户首次请求要等几秒，SSR 的首屏优势会被抵消。

常见优化方向：

- 减小函数包体积。
- 延迟初始化非关键依赖。
- 把静态资源放到 `S3 + CloudFront`。
- 让 SSR handler 只做必要渲染。
- 对高价值接口使用预热或平台提供的冷启动优化能力。
- 必要时考虑 [[边缘渲染与流式渲染]] 或自定义 SSR。

相关笔记：

- [[AWS Lambda]]
- [[Next.js 与自定义SSR]]
- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[Serverless架构]]
