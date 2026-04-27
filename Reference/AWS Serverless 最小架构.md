---
tags:
  - AWS
  - Serverless
  - 云部署
aliases:
  - AWS Serverless最小架构
  - Node SSR 上 AWS 的最小架构
status: 已整理
---

# AWS Serverless 最小架构

一个 Node SSR / API 服务落到 AWS 上，最小心智模型可以先记成：

```text
用户
-> CloudFront
-> S3 静态资源
-> API Gateway / ALB
-> Lambda
-> CloudWatch
-> SQS / MSK
```

各组件职责：

- `S3`：存放静态资源、构建产物、上传文件。
- `CloudFront`：CDN、缓存、边缘分发。
- `API Gateway`：HTTP API 入口，触发 [[AWS Lambda]]。
- `ALB`：更传统的负载均衡入口，可以和 Lambda 或容器服务组合。
- `CloudWatch`：日志、指标、告警。
- `AWS WAF`：基础防火墙。
- `SQS`：异步消息队列。
- `MSK`：托管 Kafka。

对 SSR 项目，最重要的拆分是：

```text
静态资源 -> S3 + CloudFront
动态 SSR / API -> API Gateway + Lambda
异步任务 -> SQS / MSK + Lambda
观测告警 -> CloudWatch
```

相关笔记：

- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[AWS Lambda]]
- [[AWS基础部署链路]]
- [[Serverless架构]]
