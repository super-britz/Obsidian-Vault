---
tags:
  - AWS
  - Serverless
  - Lambda
aliases:
  - Lambda
  - AWS云函数
status: 已整理
---

# AWS Lambda

[[AWS Lambda]] 是 AWS 的函数计算服务。它不是传统的常驻 Node 进程，而是由事件触发执行的 Serverless 计算单元。

传统服务更像：

```text
pm2 start -> 监听端口 -> 请求进入进程
```

Lambda 更像：

```text
事件触发 -> 平台准备运行环境 -> 执行 handler -> 空闲后回收
```

适合场景：

- SSR / API 的短请求处理。
- Webhook 回调。
- 图片压缩、文件处理。
- 队列消费。
- 定时任务。
- 轻量 BFF / 编排层。

需要关注：

- [[Lambda 冷启动]]
- [[AWS Lambda 触发器]]
- 15 分钟调用上限
- 代码包体积限制
- 运行时选择
- CloudWatch 日志与告警
- [[Serverless IaC]]

相关笔记：

- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[AWS Serverless 最小架构]]
- [[Serverless架构]]
