# 学员档案

## 基本信息

| 项目 | 内容 |
| --- | --- |
| GitHub 用户名 | OwenZhao9 |
| 钱包地址（ETH钱包，用于发奖励） | `0xe27E8193854EB6d6283AB363809d04eB93e8d957` |
| 联系方式（微信 / Telegram / Discord） | 见群内 |
| 是否加入学习交流群 | 是 |

> ⚠️ **钱包地址用于发放奖励，请务必认真填写并反复核对。** 因地址错误导致的奖励无法到账，主办方不承担责任。

## 学习笔记 / 心得

### 第一章：Avalanche 零基础入门

完成 BuilderHub 注册、Fuji 测试网水龙头领取。踩到的坑：

- 水龙头按钮显示 `Wait 23h 59m` 是 24 小时冷却，不是报错
- 钱包弹出的 `switch to Avalanche (C-Chain)` 是**主网**不是 Fuji，批准无效。Core 在测试网会明确标 `Fuji Testnet`，没这几个字就是主网
- Faucet 卡片上 `Faucet: 0.84 ALOT (low)` 后面的数字是**水龙头池子余额**，不是可领数量
- P-Chain 地址是 `P-fuji1...` 格式，和 C-Chain 的 `0x...` 不通用，P 链的 AVAX 不能直接转给 EVM 地址

### 第二章：Vibe Coding 开发第一个 DApp

用 Scaffold-ETH 2 部署 ERC20 到 Fuji 测试网，合约地址
`0xf4da661854658f03246e294fe4c2c96732f99a07`。

技术上印象最深的三件事：

**1. 合约热重载**
改完 Solidity 跑一次 `yarn deploy`，前端的 `/debug` 页面自动生成可点击的调试界面，ABI 和类型自动同步。整个过程没写一行前端代码。

**2. 读免费，写要付代价**
调试时遇到"余额有 15 ETH 但点 Send 报 `Cannot access account`"，排查后发现：地址、余额、合约状态都走公共 RPC 读取，不需要钱包签字；只有发交易改状态才需要签名和 gas。这条同时解释了 gas 机制和那个 bug 的成因。

**3. 默认部署私钥是公开的**
Scaffold-ETH 的 `hardhat.config.ts` 内置了 Hardhat 0 号账户的公开测试私钥作为 fallback。本地无所谓，但直接拿它往真实网络部署，合约 owner 就是一个人人都有钥匙的地址。部署前必须先 `yarn generate` 生成自己的加密账户。

### 遇到并解决的技术问题

| 现象 | 原因与解决 |
| --- | --- |
| `create-eth` 报 `Yarn is not installed` | Node 25 起 corepack 不再内置，`npm install -g yarn` |
| 部署到 Fuji 报 `exceeds block gas limit` | rocketh 不显式指定 gasLimit 时会继承区块上限，超过 Avalanche 单笔允许值。在 `env.deploy` 里加 `gas: 1_200_000n`（实测只需 69 万） |
| 交易成功后红色报错浮层不消失 | Next.js 开发浮层攒的是整个会话的 `console.error` 历史，不代表当前状态。判断成败要看链上数据 |

## 备注

学习过程中把踩的坑整理成了一份文档，供后来的同学参考。
