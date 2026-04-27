---
tags:
  - AWS
  - Serverless
  - Lambda
  - CloudFront
  - IaC
  - 云部署
  - SSR
aliases:
  - 29-AWS Serverless研讨｜直播笔记整理
  - 29-AWS Serverless研讨
  - AWS Serverless研讨直播笔记
  - 29附带文档整理
  - 29-SSR与Serverless补充文档
  - 29期 Serverless 补充文档
date: 2026-04-26
status: 已整理
source: 29附带文档.md
---

# 29-AWS Serverless研讨｜直播笔记整理

## 给没认真听的速读版

这节课其实是在把上一节 [[28-SSR与Serverless探讨｜直播笔记整理]] 继续往下推，重点不再是“为什么要 Serverless”，而是：

1. 现在常见的几种 Serverless 平台怎么选
2. 为什么老师最推 `AWS Lambda`
3. 一个真实的 AWS Serverless 架构里最常见的服务分别干什么
4. Lambda 到底是怎么被触发、部署、扩容和工程化管理的

如果只记 10 句话，可以先记这 10 句：

* 老师这节把 `AWS` 放在主角位，认为它仍然是 Serverless 生态里最稳、最全的一套
* `Lambda` 只是 AWS Serverless 里的“计算层”，不是全部
* 做 `Node.js SSR` 或 API 时，第一原则还是 `动静分离`
* 静态资源放 `S3`，动态请求走 `AWS Lambda`
* 对外入口最常见是 `API Gateway`，静态分发最常见是 `CloudFront`
* 安全和观测至少要考虑 `AWS WAF` 和 `CloudWatch`
* Lambda 不是端口常驻服务，它是 `事件驱动` 的
* Lambda 最重要的硬限制要记住：`15 分钟超时`、`50 MB zip / 250 MB 解压后`、`最多 5 层 Layers`
* 公司里不能靠控制台手点几百个函数，必须走 `IaC`
* 这节课最重要的落地方向就是：`AWS SAM / CDK / Terraform / SST + [[29-AWS Serverless研讨｜直播笔记整理]]-SSR与Serverless探讨｜直播笔记整理]]
* [[29-SSR与Serverless补充文档｜整理]]
* [[06-技术就业AWS讨论｜直播笔记整理]]
* [[AWS基础部署链路]]
* [[Serverless架构]]
* [[AWS Serverless 最小架构]]
* [[AWS Lambda]]
* [[Lambda 冷启动]]
* [[Serverless IaC]]
* [[双云架构]]
* [[Cloudflare 与 AWS 混合云架构]]
* [[VPC 与子网]]
* [[IGW 与 NAT Gateway]]

> [!note]
> 这份转写中间混进了不少别的声音，尤其是一些车联网/主数据页面评审内容，和这节 AWS 主线无关。我在正文里已经忽略这些串台段落，只保留和 AWS Serverless 直接相关的内容。

## 这场里需要先纠正的错词和概念

### 高概率错词

* `Clouddata / 可乐飞尔 / Clouddata worker` 基本都是 `Cloudflare / Cloudflare Workers`
* `Amazon Web service` 更标准写法是 `Amazon Web Services`，简称 `AWS`
* `Google function` 现在更准确要区分：
* `Cloud Functions` 这一名称已并入 `Cloud Run functions`
* `Cloud Run` 是更大的 Serverless 运行平台
* `server side TAG manager / SGTM` 指的是 `Server-side Google Tag Manager`
* `Openfaas` 是 `OpenFaaS`
* `call commit` 是 `CodeCommit`
* `Lana / Lama / 拉马` 基本都是 `Lambda`
* `Lambda age` 是 `Lambda@Edge`
* `close from / close front` 是 `CloudFront`
* `Interbridge` 是 `EventBridge`
* `Byterock / badrock` 是 `Bedrock`
* `AWS sum` 是 `AWS SAM`
* `cloud development kit` 是 `AWS CDK`
* `Terrafom` 是 `Terraform`
* `二模 / 2564` 基本都是 `arm64`
* `snapstart` 是 `SnapStart`

### 需要纠正的几个理解

* `Lambda` 不是 AWS Serverless 的全部，它只是计算型 FaaS；AWS 还有对象存储、队列、日志、安全、数据库等大量配套服务
* `Cloud Run` 和 `Cloud Run functions` 是 Google Cloud 现在统一后的 Serverless 体系，不能简单理解成“Cloud Run 里面套了一个 Function”
* `OpenFaaS` 是独立的开源自建 FaaS 方案，不是 Google Cloud 官方体系的一部分
* `Lambda@Edge` 不是“固定 13 个节点”的意思，更准确地说，它依托的是 `CloudFront` 的全球边缘网络
* `SnapStart` 不是所有运行时都支持的通用开关，更适合看作“某些运行时的冷启动优化选项”；`Node.js` 不是它的主适用对象
* 老师说 `Cloudflare Workers` 边缘能力比 `Lambda@Edge` 更强，这更像他的工程偏好和节点覆盖体验，不是说 `Lambda@Edge` 完全没法用

## 核心结论

* 这节课不是在教你“控制台点 Lambda”，而是在建立一套 AWS Serverless 的基础设施脑图
* 老师最看重 AWS 的地方，不只是 `Lambda` 本身，而是它周边的整套生态：`S3 / CloudFront / CloudWatch / WAF / API Gateway / SQS / MSK / EFS / EventBridge`
* 一个 Node SSR 或轻量服务真正落到 AWS 上时，最常见的第一步是 `动静分离`
* Lambda 的核心心智模型不是“端口服务”，而是“事件驱动函数”
* 你要真正会用 Lambda，就必须同时理解：
* 触发器
* 限额
* 冷启动
* Layers / 容器镜像
* 监控
* 部署方式
* 控制台手点只适合演示，不适合公司级工程。真实落地必须走 `IaC`
* 这节课里老师实际推荐的 IaC 路线有四条：`AWS SAM`、`AWS CDK`、`Terraform`、`SST`

## 一、00:31-07:52 先把平台格局讲清楚：老师为什么还是最推 AWS

### 1. 老师眼里的几类主流 Serverless 平台

开场老师先把几条线摆出来了：

* `AWS`
* `Cloudflare`
* `Google Cloud`
* 阿里云 `FC`
* 以及更少提到的腾讯云、华为云

他的判断非常直接：

* 真正最成熟、最稳定、生态最完整的，还是 `AWS`
* `Cloudflare Workers` 更像后起之秀，边缘执行很强
* `Google Cloud` 适合一些国际化和营销数据链路场景
* 其他国内云厂商也有对应方案，但老师明显不把它们放在这节课的主线里

### 2. 这节课里，Google 主要是作为“对照组”和“特殊场景补充”

老师提 Google 主要在讲两个东西：

* `Cloud Run / Cloud Run functions`
* `Server-side GTM`

他真正想表达的是：

* 如果你做国际化业务、广告归因、营销数据上报
* Google Cloud 这条线会很自然地介入

尤其是 `Server-side GTM` 的思路：

* 以前浏览器里要装很多广告/上报 SDK
* 现在可以先把事件只打到服务端容器
* 再由服务端去转发给不同平台

这和老师一直强调的思路一致：

* 浏览器侧尽量减轻
* 服务端统一收口和分发

### 3. 自建 Serverless 不是不能做，但公司体量要够

老师提到了 `OpenFaaS`，但这段的重点不是“推荐你们去自建”，而是：

* 自建 FaaS 体系不是小团队优先级
* 只有体量大、资源足、负载规模足够时才有意义

更适合当前绝大多数团队的结论仍然是：

* 先上云
* 先用现成能力
* 先把业务跑起来

## 二、07:52-10:28 这节课最重要的 AWS 服务脑图

### 1. `Lambda` 只是“计算”，不是全部

老师反复提醒：

* 大家平时说到 AWS Serverless，经常只想到 `Lambda`
* 但在真实架构里，它只是“计算型 Serverless”

AWS 在这套体系里还能提供：

* 对象存储
* CDN
* 监控
* 防火墙
* API 网关
* 负载均衡
* 消息队列
* Kafka 托管服务

### 2. 这节课里最常被点名的 AWS 服务

老师点得最多的，其实就是下面这几个：

* `AWS Lambda`：执行函数逻辑
* `S3`：存静态资源、对象文件
* `CloudFront`：CDN 分发与边缘缓存
* `CloudWatch`：监控与日志
* `AWS WAF`：防火墙
* `API Gateway`：HTTP 入口与转发
* `ALB`：应用层负载均衡
* `SQS`：消息队列
* `MSK`：托管 Kafka

这基本就是一条最小 AWS Serverless 脑图。

### 3. 老师这里反复强调“动静分离”

他在这场里把这件事讲得非常明确：

* 静态资源不要跟动态渲染混在同一个执行层里
* 静态的放 `S3`
* 动态的走 `Lambda`

这就是他要你先建立起来的第一层部署常识。

如果用更结构化的话说，就是：

* `JS / CSS / 图片 / 构建产物`：对象存储 + CDN
* `SSR / API / 动态逻辑`：函数计算

这和 [[AWS基础部署链路]] 是完全同一条线。

### 4. 安全不是“有用户再说”，而是架构的一部分

老师这里讲得很实在：

* 不是用户多了才上 `WAF`
* 而是只要是对外服务，就该考虑最基础的安全层

他真正想表达的是：

* 架构设计不能只想着“让服务跑起来”
* 还要一开始就考虑：
* 安全
* 稳定性
* 可观测性

## 三、11:14-16:27 AWS 为什么“强”，老师指的其实是生态广度

### 1. 不只是计算强，而是“什么都能接”

老师这里不是单纯夸 `Lambda` 性能，而是在强调：

* AWS 服务种类非常多
* 而且这些服务彼此之间联动得很成熟

所以他眼里的“强”是：

* 不是只有一个函数托管服务
* 而是围绕函数运行的整套生产生态

### 2. 他中间举了一个很极端的例子：卫星能力

老师这里还举到了 Amazon 的卫星网络相关能力，虽然这对大多数前端同学不是重点，但他想表达的是：

* AWS 的服务边界远不只是“网站部署”
* 它是一整套非常广的云能力集合

你在面试或工作里真正应该记住的不是“卫星”本身，而是：

* AWS 强在服务生态深
* 所以很多复杂方案可以在一个平台里闭环

## 四、16:27-25:53 Lambda 的核心心智模型：事件驱动，不是常驻端口

### 1. 传统 Node 服务的思路是“起端口”

老师用 `pm2 start` 举例，其实是在提醒大家：

* 传统服务的心智模型是：
* 进程常驻
* 端口暴露
* 域名或 IP 打到端口上

### 2. Lambda 的思路是“事件触发”

但 `Lambda` 不一样，它的核心不是端口，而是：

* `event`
* `trigger`

也就是：

* 有什么事件发生了
* 再去触发函数执行

这是理解 AWS Serverless 的第一原则。

### 3. 最常见的事件：HTTP 请求

在你做 `SSR`、API 或轻量服务时，最常见的触发方式还是：

* `API Gateway`

老师这里等于是在说：

* 以前是 `域名 -> 端口服务`
* 现在更像 `域名 -> API Gateway -> Lambda`

### 4. 其他很典型的触发器

老师现场点开创建函数时，列出了一堆触发器。对前端 / Node 方向最值得记的是这些：

* `API Gateway`
* `ALB`
* `CloudFront`
* `S3`
* `EventBridge`
* `SNS / SQS`
* `MSK`
* `CodeCommit`
* `IoT`

这里真正值得记住的不是名字本身，而是：

* Lambda 不是只接 HTTP
* 任何能被定义成“事件”的事情，都有可能成为它的入口

## 五、18:47-25:53 课堂上举的三个非常典型的触发器场景

### 1. `S3 Trigger`：图片裁剪、压缩、格式转换

老师这里举了一个很典型的图床 / 媒体处理例子：

* 用户把图片传到 `S3`
* `S3` 事件触发 `Lambda`
* `Lambda` 做：
* 裁剪
* 压缩
* 格式转换

例如：

* 原图是 `PNG`
* 体积很大
* 最后转成更适合前端分发的 `WebP`

这个例子很经典，因为它特别能体现 Serverless 的价值：

* 不需要长期养一个图片处理服务
* 有上传事件时再执行

### 2. `CloudFront Trigger / Lambda@Edge`：做边缘执行

老师这里的重点是：

* 把 Lambda 和 CDN 绑起来
* 就能把一部分逻辑放到离用户更近的地方执行

也就是：

* 请求先进 `CloudFront`
* 在边缘侧就触发逻辑
* 再决定如何返回内容

这里他的重点不是讲 Lambda@Edge 的 API 细节，而是说明：

* 边缘执行可以直接拿来缩短跨地域时延

### 3. `EventBridge`：定时任务和保活

老师把 `EventBridge` 放在两个语境里讲：

* 做定时任务
* 做保活 / keep-warm

后面他举的实际做法是：

* 每 5 分钟用 `EventBridge` 去调一次目标 Lambda
* 让函数环境保持活跃，减少冷启动影响

这件事本质上就是“用调度换更稳定的首响应”。

## 六、26:15-34:42 Lambda 的限制和可调参数，这些很适合面试背

### 1. 最重要的硬限制：单次执行最长 15 分钟

老师说得非常明确：

* 这是底层硬限制
* 超了就会超时并被中断

所以 `Lambda` 很适合：

* 短任务
* API
* SSR
* 网关转发
* 图像处理

但不适合：

* 特别长的长任务
* 持续运行的常驻服务

### 2. 包大小限制

这节课里提到的两个关键数值是：

* `zip` 上传包大小限制
* 解压后的总体积限制

更实用地记就是：

* 包太大时，先想是不是依赖问题
* 再考虑：
* `Layers`
* 容器镜像

### 3. `Layers` 是在帮你拆分依赖，不是在给你无限扩容

老师这里讲得挺实在：

* 你手写代码本身一般不会大到夸张
* 真正把包搞大的，经常是三方依赖

所以：

* 可以把共用依赖拆到 `Layers`
* 但 `Layers` 自己也有限制
* 而且不是无限叠加的

它更像是：

* 给运行时和依赖做一层共享装载
* 不是让你无脑往里塞一切

### 4. 包再大或运行时不支持时，就上容器镜像

老师这里给出的两个典型场景是：

* 包太大
* 官方运行时不支持你的语言/环境

这时就该考虑：

* `container image`

也就是：

* 直接把函数做成容器镜像
* 由你自己定义运行环境

老师还顺手举了：

* `Deno`
* 自定义语言
* 编译器/运行时场景

他想表达的是：

* Lambda 不只是“官方支持几门语言”
* 真到需要时，容器镜像会把边界拉开很多

### 5. 内存、架构、短暂存储、EFS 都是重要调优项

老师在控制台里点到的常见调优项包括：

* 内存
* 架构
* `/tmp` 这类临时存储
* `EFS`

他的经验判断大概是：

* 内存别卡太死，给高并发留余地
* `arm64` 值得优先考虑
* EFS 能解决一部分持久化/共享文件问题
* 但 EFS 也会拖长冷启动

这里的工程常识其实很重要：

* 任何“附加能力”都可能换来更慢的初始化
* Lambda 调优从来不是只看一个开关

### 6. 冷启动和 `SnapStart`

老师先花了一段解释冷启动：

* 容器起来
* 运行时初始化
* 代码从存储加载
* 依赖初始化
* 之后函数才真正开始执行

然后又讲到 `SnapStart`，他的核心表达是：

* 对某些运行时，它可以显著降低冷启动
* 尤其适合启动慢的语言

更准确地整理就是：

* `SnapStart` 是针对部分运行时的冷启动优化
* 不是所有运行时都能直接开
* 它本质上是“提前拍快照，下次复用初始化结果”

## 七、40:07-50:50 公司级落地不能靠手点，必须上 IaC

### 1. 控制台手动创建只适合演示

老师这里直接现场创建了一个 `Hello from Lambda`，目的其实不是教你点控制台，而是为了说明：

* 手点可以证明“这服务能跑”
* 但在公司里不可能这么做

原因很简单：

* 服务太多
* 环境太多
* 重复劳动太多
* 出错概率太高

### 2. 这就是 `IaC` 的意义

老师这里把 `IaC` 讲得很直白：

* 用代码描述基础设施
* 用配置文件去创建、更新、删除资源
* 而不是靠人手动点界面

这不只是为了“高级”，而是为了：

* 可复用
* 可审计
* 可 review
* 可批量管理

### 3. 老师这节提到的四条 IaC 路线

#### `AWS SAM`

它更偏：

* 以 Serverless 为中心
* 用模板快速定义 `Lambda + API Gateway + 相关资源`

老师现场让 AI 直接生成了一份最小可用配置：

* 全局配置
* 函数定义
* handler 入口
* 路由 path
* 输出 API 地址

这其实就是在告诉大家：

* 对简单 Serverless 项目，`SAM` 已经足够实用

#### `AWS CDK`

它更偏：

* 用常规编程语言写基础设施

老师特别喜欢它的一点是：

* 你可以用 `TypeScript / JavaScript / Python / Go / Java / Rust` 等熟悉语言去描述资源

这意味着：

* IaC 不再只是配置文件
* 它也可以像正常代码一样封装、复用、抽象

#### `Terraform`

老师这里把它放在“跨云”的位置上讲：

* 如果你不想只锁在 AWS 一家
* 想管多云、多平台
* `Terraform` 会更通用

#### `SST`

老师这里把 `SST` 讲成：

* 建在 `Terraform` 之上的更友好的封装
* 对前端 / Node / Serverless 开发者更顺手

但他也提醒：

* 它对系统环境比较敏感
* 尤其非 Linux / macOS 环境会更容易踩坑

## 八、51:19-59:59 IaC 怎么接 CI/CD，以及这节后面给出的学习方法

### 1. IaC 不会替代 CI/CD，它是被 CI/CD 执行的对象

老师这段讲得很清楚：

* 无论你用的是 `Jenkins`
* `GitHub Actions`
* `GitLab CI`
* 还是 `CodePipeline`

本质上都只是：

* 在流水线里执行 IaC 命令

也就是说：

* CI/CD 是“怎么跑”
* IaC 是“跑什么”

### 2. 架构图不是为了背组件名，而是练习“怎么把系统拆层”

老师后面展示了几张典型架构图：

* `OpenNext` 风格的 Next.js on AWS 架构
* 监控系统架构
* Lambda 互调的微前端 / 公共 Header 渲染方案

他真正想让大家看懂的不是“这图用了多少 AWS 图标”，而是：

* 用户从哪里进来
* CDN 怎么分流
* 静态和动态怎么拆
* 缓存层怎么接
* 异步层怎么接
* Lambda 和 Lambda 如何解耦协作

### 3. 老师给出的学习路线很明确

他最后其实给了很清楚的学习方法：

* 看 `AWS blog`
* 看 `Uber / Meta / Netflix` 等工程博客
* 不要把架构理解成“前端专属话题”
* 真正的架构思维是全栈和系统层的

他特别强调：

* 现在 AI 已经能把文档和博客讲得很清楚
* 真正重要的是你能不能把它转化成自己的工程判断

## 九、这节里最值得你带走的几个工程判断

### 1. Node SSR 上 AWS 的第一张最小图

如果把老师这节压成一张最小 AWS Serverless 图，大概就是：

* 用户请求进入 `CloudFront`
* 静态资源直接走 `S3`
* 动态请求转给 `API Gateway`
* `API Gateway` 触发 `Lambda`
* 监控日志进 `CloudWatch`
* 安全由 `WAF` 兜底

### 2. 什么场景更适合 `Lambda`

* SSR
* 短 API
* 事件处理
* 图片处理
* 定时任务
* 中轻量异步处理

### 3. 什么场景更不适合 `Lambda`

* 超长任务
* 长连接
* 特别重的常驻服务
* 强依赖本地持久状态的任务

这种情况下，老师更偏向：

* `EKS`
* `ECS`
* 常规容器服务

### 4. 什么才是这节课最重要的面试回答

如果面试官问你：

* AWS 上一个 Node SSR / API 服务怎么落地？

你至少要能说清：

* 动静分离
* `S3 + CloudFront`
* `API Gateway + Lambda`
* `CloudWatch + WAF`
* 事件驱动
* 冷启动
* `IaC`
* CI/CD

## 主线外的插曲，可以略读

这份转写里还有几段不属于 AWS 主线的内容，可以知道有这么回事，但复习时不用放太多精力：

* 老师插了一段关于 AI 模型聚合平台和 Claude 账号风控的讨论
* 中间混进了另一位说话人关于车联网/主数据页面交互的讨论
* 这些都不是这节 AWS Serverless 的关键知识点

## 相关笔记

* [[28-SSR与Serverless探讨｜直播笔记整理]]
* [[06-技术就业AWS讨论｜直播笔记整理]]
* [[AWS基础部署链路]]
* [[Serverless架构]]
* [[双云架构]]
* [[Cloudflare 与 AWS 混合云架构]]
* [[VPC 与子网]]
* [[IGW 与 NAT Gateway]]

# 29-SSR与Serverless补充文档｜整理

这份补充文档是在 [[29-AWS Serverless研讨｜直播笔记整理]] 之外，对 [[SSR]]、[[Serverless架构]]、[[AWS Lambda]] 和 [[Serverless IaC]] 的提纲式补充。

它真正要补的不是某个 API，而是一个部署判断：

> 当 SSR / Node 服务从“能跑”进入“要高并发、低成本、可观测、可降级”阶段时，部署方式就不能停留在单机或固定 ECS/EC2 上，而要认真评估 Serverless。

## 1. SSR 先解决什么问题

文档里把 SSR 的目的压成两类：

1. `SEO / AIO`
2. 性能

`SEO / AIO` 指的是页面要能被搜索引擎、ClaudeBot、GPTBot 等抓取。以前只说 SEO，现在还要考虑 AI 检索和内容索引。相关笔记是 [[AIO 与 SEO]]。

性能侧主要看 [[TTFB 与 Web Vitals]]。SSR 不是天然更快，它只是让首屏 HTML 可以更早返回；真正的效果取决于服务端渲染耗时、缓存、CDN、数据请求和部署方式。

## 2. 为什么会考虑自定义 SSR

默认情况下，React 项目可以先选 `Next.js`。但补充文档列了几个会逼团队考虑 [[Next.js 与自定义SSR]] 的原因：

- 冷启动：项目代码变大后，Next.js 在 Serverless 场景下可能出现很重的冷启动。
- 安全性：大框架能力多，历史高危漏洞和供应链风险也要关注。
- 稳定性：需要能对 `4xx / 5xx` 做降级，比如回退到静态 HTML。
- 扩展性：SRI nonce、中间件上下文、微前端、平台绑定等问题可能卡住业务。
- 老项目迁移：例如多年 CSR / CRA 项目要补 SSR，但不想推翻重写。

所以自定义 SSR 的判断标准不是“想不想炫技”，而是：

```text
框架便利性 < 冷启动 / 安全 / 稳定性 / 迁移成本 / 扩展性
```

相关笔记：

- [[SSR 老项目迁移]]
- [[边缘渲染与流式渲染]]
- [[Lambda 冷启动]]

## 3. Serverless 的核心心智

补充文档里对 Serverless 的总结很实用：

- 动态自动扩缩容
- 平台托管监控、日志、容灾、告警
- 按调用次数、CPU 计算时间、内存计费
- 通过 IaC 快速迭代

传统方式像：

```text
买 ECS / EC2 -> 部署常驻 Node 服务 -> 自己扩容 -> 自己监控
```

Serverless 更像：

```text
事件触发 -> 平台拉起函数 -> 执行业务 -> 空闲后回收
```

这解释了为什么它适合 SSR、API、Webhook、图片处理、异步任务，也解释了为什么它不适合长连接常驻、超长任务、强本地状态依赖的服务。

## 4. 云厂商和平台选择

补充文档提到几类平台：

- [[AWS Lambda]]
- `Cloudflare Workers`
- `Vercel`
- `Google Cloud Run`
- 阿里云 `FC / Edge Routine`
- `OpenFaaS`

简单判断：

| 平台 | 更适合什么 |
| --- | --- |
| AWS Lambda | 成熟云生态、企业级 Serverless、和 S3/CloudFront/SQS/CloudWatch 组合 |
| Cloudflare Workers | 边缘执行、全球节点、轻量 API、边缘渲染 |
| Vercel | Next.js / 前端团队快速上线 |
| Google Cloud Run | 容器化 Serverless、sGTM、Google 营销数据链路 |
| 阿里云 FC | 国内云环境 |
| OpenFaaS | 有足够工程能力时自建 FaaS |

## 5. AWS Serverless 最小架构

文档列出的 AWS 组件可以整理成这条最小链路：

```text
用户
-> CloudFront
-> S3 静态资源
-> API Gateway / ALB
-> Lambda
-> CloudWatch 日志监控
-> SQS / MSK 异步消息
```

常见职责：

- `S3`：对象存储，放静态资源。
- `CloudFront`：CDN 和边缘分发。
- `CloudWatch`：日志、指标、告警。
- `AWS WAF`：防火墙和基础安全防护。
- `API Gateway`：HTTP API 入口。
- `ALB`：负载均衡入口。
- `SQS / MSK`：消息队列和 Kafka 托管。
- `Bedrock`：AI 能力入口。
- `EC2 Auto Scaling / EKS`：不是 Serverless 核心，但常在混合架构里共存。

相关卡片：[[AWS Serverless 最小架构]]。

## 6. Lambda 的调用方式

传统 Node 服务是：

```text
pm2 start -> 监听端口 -> 通过 IP / 域名访问
```

Lambda 是：

```text
事件 / Trigger -> 拉起函数 -> 执行 handler
```

常见触发来源：

- `API Gateway`：HTTP `GET / POST / PUT / OPTIONS`
- `S3`：对象上传后触发，比如图片压缩
- `CloudFront / Lambda@Edge`：边缘请求处理
- `CloudWatch / EventBridge`：定时任务、日志事件
- `SQS`：队列消息消费
- `MSK`：Kafka 消息消费

相关卡片：[[AWS Lambda 触发器]]。

## 7. Lambda 的限制要记住

补充文档里最值得背的限制：

- 单次调用最长 15 分钟。
- zip 包不超过 50 MB。
- 解压后不超过 250 MB。
- Layers 每个函数最多 5 个。
- Layers 也受 50 MB zip / 250 MB 解压限制。
- 可以用自定义容器提升扩展性。

这几个限制会影响 SSR 项目的依赖体积、构建产物拆分、二进制依赖、图片处理和运行时选择。

## 8. Runtime 和冷启动

常见 Runtime：

- Node.js
- Python
- Go
- Rust
- Java

冷启动的核心是：函数长时间无人调用后，平台需要重新准备运行环境、加载代码、初始化依赖。代码越大、运行时越重、初始化逻辑越多，冷启动越明显。

相关卡片：[[Lambda 冷启动]]。

## 9. IaC 是公司级落地前提

补充文档列了四条 IaC 路线：

- `AWS SAM`
- `AWS CDK`
- `Terraform`
- `SST`

它们解决的是同一个问题：

```text
不要手点控制台，而是用代码定义基础设施
```

公司级 Serverless 项目需要把 Lambda、API Gateway、S3、CloudFront、IAM、环境变量、日志、告警、队列等资源一起版本化和自动化部署。

相关卡片：[[Serverless IaC]]。

## 10. 和前端学习的关系

这份文档对前端的价值在于：它把“写页面”往“应用上线和架构承载”推了一层。

你需要能回答：

- SSR 为什么不只是 SEO？
- 为什么 SSR 会带来服务端压力？
- 为什么 Serverless 适合 SSR / API？
- Lambda 和传统 Node 服务有什么不同？
- 冷启动为什么是面试关键词？
- 为什么公司不能靠手点云资源？
- 静态资源、动态渲染、异步任务分别应该放在哪里？

一句话总结：

> 29 期补充文档的重点是：把 SSR 从框架问题，升级成部署、扩容、成本、可观测和 IaC 的工程问题。

## 相关笔记

- [[29-AWS Serverless研讨｜直播笔记整理]]
- [[28-SSR与Serverless探讨｜直播笔记整理]]
- [[23-SSR框架技术讨论｜直播笔记整理]]
- [[Serverless架构]]
- [[AWS Serverless 最小架构]]
- [[AWS Lambda]]
- [[AWS Lambda 触发器]]
- [[Lambda 冷启动]]
- [[Serverless IaC]]
- [[Next.js 与自定义SSR]]
- [[边缘渲染与流式渲染]]
