---
tags:
  - 前端性能
  - Web Vitals
  - TTFB
aliases:
  - TTFB与Web Vitals
  - 首字节时间与核心性能指标
status: 已整理
---

# TTFB 与 Web Vitals

这场课里老师把性能分成两层：

1. `TTFB`
2. `Web Vitals`

`TTFB` 是：

* 从请求发出到服务端返回首字节的时间

老师为什么强调它：

* 流式渲染最直接优化的就是这个指标
* 首字节越早，用户和 BOT 越早开始看到内容

他还强调了核心性能指标至少要做到 `good`，尤其是：

* `LCP`
* `INP`
* `CLS`

在这场课的语境里，这些指标不只是影响用户体验，也影响：

* [[AIO 与 SEO]]
* BOT 抓取效率
* 页面整体权重

相关笔记：

* [[23-SSR框架技术讨论｜直播笔记整理]]
* [[边缘渲染与流式渲染]]
* [[SSR 与 CSR]]

