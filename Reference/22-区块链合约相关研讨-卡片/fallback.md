---
tags:
  - Solidity
  - 回调
source_note: "[[22-区块链合约相关研讨-整理笔记]]"
---

# fallback

`fallback()` 可以理解成“兜底回调”。

这节课里的课堂语境是：

- 如果交易带了数据
- 或者不匹配已有函数
- 那就可能落入 `fallback()` 一类逻辑

它经常和 [[receive]] 一起被理解。

相关笔记：

- [[receive]]
- [[数据上链]]

