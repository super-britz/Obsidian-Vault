# AWS基础部署链路

这页用来把这节课里最重要的一条部署链压缩成一张最小脑图。

## 最小链路

代码 -> `sam deploy` -> `CloudFormation` -> `IAM Role` -> `API Gateway` -> `Lambda` -> `VPC / Subnet / Security Group` -> `Aurora / S3 / ElastiCache / SQS`

## 每一层大概负责什么

### `sam deploy`

* 把本地代码打到 AWS 的入口命令之一
* 老师这一场把它当成部署主线的起点

### `CloudFormation`

* 资源编排层
* 帮你创建和绑定一整套 AWS 资源
* 不只是“把代码放上去”

### `IAM Role`

* 权限层
* 谁能访问什么、谁能部署什么，很多都靠它驱动

### `API Gateway`

* 对外入口
* 接住外部请求，再把流量转给内部服务

### `Lambda`

* 执行层
* 承接函数式、按请求触发的逻辑

### `VPC / Subnet / Security Group`

* 网络隔离层
* 决定你的服务、数据库、缓存到底怎么互通

### 数据与异步层

* `Aurora / RDS`：数据库
* `S3`：对象存储
* `ElastiCache`：缓存
* `SQS`：队列

## 这条链里最容易卡住的地方

* `Role` 权限不对
* Lambda 进入 VPC 后网络不通
* 子网和安全组没有配对
* 私有子网没有正确出网

## 这页最重要的一句话

AWS 部署不是“传代码”，而是“让代码落到一整套资源关系里”。

相关笔记：

* [[06-技术就业AWS讨论｜直播笔记整理]]
* [[VPC 与子网]]
* [[IGW 与 NAT Gateway]]
* [[Serverless架构]]
