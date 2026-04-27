---
tags:
  - Serverless
  - IaC
  - AWS
  - DevOps
aliases:
  - IaC
  - Serverless基础设施即代码
status: 已整理
---

# Serverless IaC

[[Serverless IaC]] 指用代码定义和部署 Serverless 基础设施，而不是在云控制台里手点资源。

它要解决的问题：

```text
Lambda / API Gateway / S3 / CloudFront / IAM / SQS / CloudWatch
这些资源必须能版本化、复用、审查、回滚、自动部署
```

常见路线：

- `AWS SAM`：AWS 官方 Serverless Application Model，适合快速定义 Lambda + API Gateway。
- `AWS CDK`：用 TypeScript、Python、Go 等语言定义云资源，底层生成 CloudFormation。
- `Terraform`：跨云通用 IaC，适合多云和平台团队。
- `SST`：更偏现代 Serverless 应用开发体验，对前端 / Node 团队友好。

IaC 和 CI/CD 的关系：

```text
IaC 定义基础设施
CI/CD 执行部署流程
```

公司级项目不能依赖控制台手点，因为手点无法稳定复现，也很难审计和回滚。

相关笔记：

- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[AWS基础部署链路]]
- [[Serverless架构]]
