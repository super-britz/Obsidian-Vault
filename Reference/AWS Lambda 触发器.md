---
tags:
  - AWS
  - Lambda
  - Serverless
aliases:
  - Lambda触发器
  - Lambda Trigger
status: 已整理
---

# AWS Lambda 触发器

[[AWS Lambda]] 的核心不是监听端口，而是被事件触发。这个触发来源就叫 Trigger。

常见触发器：

- `API Gateway`：HTTP API 入口，处理 `GET / POST / PUT / OPTIONS`。
- `ALB`：负载均衡入口，也可以把请求转给 Lambda。
- `S3`：对象上传、删除等事件触发文件处理。
- `CloudFront / Lambda@Edge`：边缘请求处理。
- `CloudWatch / EventBridge`：定时任务、系统事件、日志事件。
- `SQS`：消费队列消息。
- `MSK`：消费托管 Kafka 消息。

理解触发器的价值：

```text
Lambda 不是一个服务，而是一段可被不同事件入口复用的计算逻辑
```

所以同一个 Serverless 系统里，HTTP 请求、文件上传、队列消息、定时任务都可以用统一的函数计算模型承接。

相关笔记：

- [[AWS Lambda]]
- [[AWS Serverless 最小架构]]
- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[AWS基础部署链路]]
