# 展示 2020 Unitize 黑客马拉松的获胜者

> 原文：<https://blog.chain.link/showcasing-the-winners-of-the-2020-unitize-hackathon/>

我们很荣幸成为 2020 Unitize Hackathon 的七家赞助商之一，为在新颖的[智能合约](https://chain.link/education/smart-contracts)应用设计中集成 Chainlink oracles 的区块链开发者提供总计 5000 美元的链接奖金。我们收到了许多 Chainlink 提交的作品，展示了从企业元交易协议和智能合同双因素认证到时间锁定储蓄账户和区块链俄罗斯方块游戏的广泛创意。

最后，我们颁发了 12 个奖项，其中包括 10 个展示如何使用 [Chainlink 价格参考数据](https://data.chain.link/)的项目，以及两位主要获奖者，他们展示了 Chainlink 推动普遍连接智能合同的独特潜力，以及在现实世界中的强大应用。

## 综合最佳奖:林克加油站

1500 美元的林克大奖颁给了[林克加油站](https://github.com/pappas999/Link-Gas-Station)，这是哈里·帕帕查里西乌的一个个人项目。企业采用智能合同的一个障碍是围绕获取、持有和保护加密货币/令牌的复杂性和不确定的监管环境，这些加密货币/令牌是与它们进行交互(例如支付汽油)所必需的。LINK Gas Station 通过抽象出企业持有加密货币的需求来绕过这些限制，允许它们甚至在监管框架跟上之前与下一代分散式应用程序进行交互。

LINK Gas Station 采用了[元交易](https://defirate.com/meta-transactions/)的概念——区块链交易费被提取出来，由一个继电器支付——并将其应用于 Chainlink。它使用第三方中继器来管理实用令牌 LINK 和 ETH 的所有权，这两者都是支付以太坊计算和获得 Chainlink [oracle](https://chain.link/education/blockchain-oracles) 数据服务所必需的。通过这样做，加密货币所有权的责任和复杂性从企业转移到了所选择的继电器上，导致企业可以简单地用菲亚特支付发票，并获得整个分散生态系统的访问权。重要的是，企业仍然完全控制签署交易所需的加密私钥。

通过利用 LINK 的本机 [ERC677](https://github.com/ethereum/EIPs/issues/677) transferAndCall()和 onTokenTransfer()功能，中继者可以在一次交易中代表企业支付和请求外部数据。同时，中继站还轮询 [Chainlink 价格参考源](https://data.chain.link/)，以生成交易中使用的费用(ETH 和 link)的固定价值的链上承诺。最终结果是一个系统，在该系统中，与链式智能合同进行交互的企业不再需要持有 LINK 或 ETH，因为中继代表他们进行支付，并在生成固定发票时创建这些请求成本的无可辩驳的链上证明。

Harry 谈到了设计背后的灵感，指出“企业 IT 模式已经从拥有硬件、服务器和软件转变为向服务提供商付费，并根据许可证或使用情况作为运营费用收取费用。看看智能合同(特别是链式智能合同)，我认为从企业 IT 的角度来看，没有理由不能以同样的方式对待它们。”

他还提到了集成 Chainlink 的便利性，称“连接到常用的链外数据，如价格或天气数据，就像插入几行代码一样快速简单，而 Chainlink 节点的外部适配器功能使其足够强大，可以构建几乎任何链外集成。”

## 评委最佳选择奖:智能合同双因素认证

评委的最佳奖项授予了由 Javier Salomón、Mateo Hepp 和 Alejandro Pronotti 组成的三人团队 [Digital Bridge](http://digitalbridge.sismatech.com.ar/) 。双因素身份验证(2FA)是额外的安全层，除了密码之外，用户还必须从只有他们有权访问的可信设备上确认代码或单击该设备上的按钮。

鉴于安全性对智能合约的极端重要性，Digital Bridge 通过开发一个用于读取离线设备的 [Chainlink 外部适配器](https://docs.chain.link/docs/external-adapters)和设置一个负责转发确认 2FA 所需密码的 Chainlink 节点，将 [2FA 应用于智能合约](https://github.com/apronotti/2fa-for-smartcontracts)中。为了认证持有 2FA 私钥的用户，用户提交一个包含他们的 ID 和由他们的认证器应用生成的临时一次性密码的链上交易。然后，扫描区块链的链节点发现这个事务，并查询链外服务器来验证 2FA 代码的真实性。一旦 Chainlink 节点接收到响应，它就将这个布尔值传递到链上，然后链上确定智能合约是否将授权授予原始用户。

由于时间限制，Digital Bridge 的团队计划通过使用 PIN 的 SHA256 散列并创建一个外部适配器来比较它们，来扩展这一功能。这提供了增强的安全性，并允许在没有中间人攻击的情况下在生产中使用这种方法。通过使智能合约能够访问 2FA 确认，可以创建深度防御策略来进一步保护分散应用程序中的用户资金。

Digital Bridge 明确表达了他们构建它以与现实世界对接的意图，称“考虑到第二层安全的重要性，我们开发了与 Google Authenticator、Auth0、Microsoft Authenticator、Authy、Aegis Authenticator 等兼容的智能合约 2FA。”当被问及他们使用 Chainlink 的体验时，他们评论了他们得到的开发人员支持，表示“我们喜欢评论 Chainlink 项目有多么好的记录以及社区解决问题的良好倾向。”

## 亚军奖项

除了两个主要奖项之外，我们还向具体使用 Chainlink 价格参考数据的 10 名参与者每人颁发了 250 美元的 LINK 奖金:

*   ****[**俄罗斯方块**](https://github.com/mul1sh/tetrion)**–******一款基于区块链的俄罗斯方块游戏，用户可以通过下注足够的 ETH 来加入房间，最终奖励给持续时间最长的人。Chainlink 价格参考数据决定该人是否有足够的 ETH 加入房间，而 Chainlink VRF 确保房间 id 的唯一生成，以防止干扰现有的房间。
*   ****[**crypphecy**](https://github.com/DrNguyen2525/Crypto_Supreme_Prophecy)**–****一种二元期权游戏，用户试图预测一小时后 BTC 的价格变化，使用 Chainlink 价格馈送进行结算。**
***   ****[**Bridgify**](https://github.com/viraj124/Bridgify)**–****dApp 作为 [DeFi](https://chain.link/education/defi) & CeFi 之间的桥梁，用户可以根据 Chainlink 的实时价格将其 DAI 兑换成欧元、英镑或日元。*****   ****[**Chainlink Reference**](https://github.com/azizyano/chainlink-reference-price-eth-usdl)**–******基于 chain link 的参考价格进行以太币稳定币和以太币交换的 dApp*   [**小猪**](https://github.com/colinbowen/piggy)**–**一个可以锁定时间的全球互助储蓄账户，资产余额根据 Chainlink 价格反馈显示*   ****[**懒惰交易机器人**](https://github.com/Usman75/Lazy-trader-bot)**–******使用 Chainlink 的 ETH/USD 价格参考源自动进行 DAI/ETH 互换的机器人*   [**赌王**](https://github.com/vinhyenvodoi98/ChainLink_Unitize_-SFBW-_Hackathon)**–**一种二元期权游戏，用户试图预测三个小时后的 ETH 价格变化，使用 Chainlink 价格源结算。*   ****[**加密卡**](https://github.com/littlezigy/crypto-cards)**–****一种基于区块链的二十一点游戏，使用链接价格馈送允许玩家将他们的奖金兑现到戴*****   [**LINK 加油站**](https://github.com/pappas999/Link-Gas-Station)——因为它也使用了 Chainlink 价格参考数据，所以它有资格获得这项额外奖励******

 **我们感谢所有 2020 Unitize Hackathon 参与者使用 Chainlink 进行建设，并期待继续参与未来的 Hackathon，以推进我们的使命，即让普遍连接的智能合同成为全球数字协议的主导形式。

## **今天就用 Chainlink 开始建造**

想自己动手做点什么或者了解更多关于 Chainlink 的知识？请务必立即前往 [Chainlink 文档](https://docs.chain.link/docs/getting-started)以构建外部连接智能合约，并参加 [Discord](https://discordapp.com/invite/aSK4zew) 中的技术讨论。如果您想安排一次电话会议，更深入地讨论整合事宜，请点击此处的。

我们还有一个正在进行的 [Chainlink 社区资助计划](https://blog.chain.link/introducing-the-chainlink-community-grant-program/)和 [Gitcoin Bounty 计划](https://gitcoin.co/issue/smartcontractkit/chainlink/3239/100023497)，面向那些为 Chainlink 生态系统构建关键基础设施和/或对 Chainlink 开源开发感兴趣的人。

[网站](https://chain.link/) | [推特](https://twitter.com/chainlink)|[Reddit](https://www.reddit.com/r/Chainlink/)|[YouTube](https://www.youtube.com/channel/UCnjkrlqaWEBSnKZQ71gdyFA)|[电报](https://t.me/chainlinkofficial) | [事件](https://blog.chain.link/tag/events/) | [GitHub](https://github.com/smartcontractkit/chainlink) | [价格供稿](https://feeds.chain.link/) | [DeFi](https://defi.chain.link/)**