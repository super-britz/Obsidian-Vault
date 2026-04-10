# Solidity 与合约基础

这页主要从 [[21-区块链课程等讨论｜直播笔记整理]] 拆出来，用来承接 Solidity 入门和合约最小工作流。

## 一句话理解

`Solidity` 语法本身不难，真正难的是：

* 安全
* `gas`
* 资金流转规则
* 通证经济设计

## 最小工作流

老师这场给的最小链路是：

1. 在 `Remix` 新建 `.sol` 文件
2. 写 `pragma`
3. 写 `contract`
4. 编译
5. 部署
6. 拿到交易哈希和合约地址
7. 前端再围绕这个地址去调用

## 最值得先记住的概念

### 状态变量

老师会把它讲成“世界状态树”的一部分，核心意思就是：

* 它不是只存在本地
* 它是链上状态
* 读和写的成本不一样

### gas

可以先粗暴理解成：

* `gas price` = 单价
* `gas limit` = 最多消耗多少

写状态、部署合约、执行交易都会和它有关。

### payable

只要函数涉及：

* 收钱
* 转钱

就要重点看它是不是 `payable`。

### msg / block

最常见的是：

* `msg.sender`
* `msg.value`
* `block` 里的区块信息

### memory / storage

这是 Solidity 入门里最该先分清的一组：

* `memory` 更像临时值
* `storage` 更像真的改链上状态

## 这场课里点到的高频语法

* `contract`
* `constructor`
* `public / private / internal / external`
* `view / pure`
* `constant`
* `payable`
* `mapping`
* `struct`
* `interface`
* `modifier`
* `require`
* `assert`

## 老师的现实判断

老师强调两件事：

* 前端不一定马上转合约，但必须能看懂合约
* 直接找合约岗很难，更现实的路是先进入 Web3 团队再转岗

## 适合记住的一句话

> 合约代码不一定长，但它控制的是规则、钱和风险。

相关笔记：

* [[21-区块链课程等讨论｜直播笔记整理]]
* [[Web3基础概念]]
* [[公链与代币]]
* [[Web3钱包与测试网]]
