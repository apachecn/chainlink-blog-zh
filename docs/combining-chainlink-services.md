# 结合多个 Chainlink Oracle 服务构建高级智能合同

> 原文：<https://blog.chain.link/combining-chainlink-services/>

chain link Network 为智能合同开发者提供了广泛的 oracle 服务，这些服务为他们的应用程序提供了[](https://blog.chain.link/understanding-how-data-and-apis-power-next-generation-economies/)和 [高级计算能力](https://blog.chain.link/what-is-oracle-computation/) 。Chainlink oracle 服务旨在推动智能合约经济新兴行业的发展。例如，[chain link Price Feeds](https://chain.link/data-feeds)使 DeFi 应用程序能够根据实时市场状况动态管理资产，而 [Chainlink 可验证随机函数](https://chain.link/chainlink-vrf)【VRF】使 NFTs 和区块链游戏能够创建公平的分配模式和不可预测的游戏玩法。

链上[](https://blog.chain.link/what-is-a-blockchain-and-how-can-it-impact-the-world/)基础设施和链下[oracle](https://chain.link/education/blockchain-oracles)服务的这种组合形成了一个强大的新 [混合智能契约](https://blog.chain.link/hybrid-smart-contracts-explained/) 框架的基础，在该框架中，应用程序可以保留区块链的非托管和抗审查特性，同时通过 Oracle 变得更加丰富和高效。尽管许多混合智能合同都是从使用单一 oracle 服务开始的，但随着应用程序变得越来越复杂，这种情况正在迅速改变。现在，开发人员正在将多个 Chainlink oracle 服务结合在一个应用程序中，以释放更多的效用并简化用户体验。

在这篇博文中，我们通过三种初始组合的视角来探讨 Chainlink oracle 服务的互补性:价格反馈+自动化、VRF +自动化和 CCIP +价格反馈。

![Multiple Chainlink Oracle Services](img/d5740df00e6eb9f1fcb18ea570c4c9a6.png)

<figcaption id="caption-attachment-2858" class="wp-caption-text">Combining Multiple Chainlink Oracle Services</figcaption>



### 价格反馈+自动化

DeFi 应用程序在采取特定的链上行动时通常需要实时价格数据，例如确定用户的最大贷款规模或计算期货合约的支出。随着 DeFi 成为智能合约的第一大板块， [Chainlink 价格馈送](https://data.chain.link) 推出。chain link Price feed 已成为区块链最广泛使用的 oracle 解决方案，可提供准确、防篡改的金融市场数据，700 多个 chain link Price feed 已在许多领先的区块链网络中运行。

然而，智能合约在默认情况下不是自主的；它们需要一个外部触发事务来指示它们何时执行某些链上操作。例如，一份期权合约只有在得到通知后才会结算。挑战在于确保在特定条件下负责触发应用程序的机制高度可靠。为了消除任何集中的故障点，Chainlink Automation 应运而生。 [Chainlink Automation](https://chain.link/automation) 是一款分散式交易自动化解决方案，代表智能合同和开发团队执行 DevOps 任务。在许多 DeFi 用例中，Chainlink Automation 经常与 Chainlink Price Feeds 结合使用，即自动化节点触发智能合同流程，该流程在执行期间依赖于价格 feed。

举一个这样的例子， [Siren](https://siren.xyz/) 使用 Chainlink 价格馈送和 Chainlink 自动化来帮助增强其 DeFi 选项协议。价格馈送为协议提供安全的价格数据，用于准确的期权定价和结算，自动化用于帮助在到期时可靠地执行期权。

当期权合约到期时，Siren 协议需要执行结算过程。此外，当期权到期时，Siren 需要设定到期日的结算价。Chainlink 价格馈送和自动化的结合有助于确保期权结算价格始终准确，并确保期权在期权到期后不久就能可靠地按时结算。 通过结合多个 Chainlink oracle 服务，Siren 提高了可靠性，降低了期权结算和定价等关键协议功能的延迟，极大地改善了分散化应用的用户体验。

### 可验证的随机性+自动化

继 DeFi 之后，NFTs 和区块链游戏公司已经成为最新的基于智能合约的行业，以实现用户采用。贯穿两者的一个关键要素是随机性，它被用来产生不可预测性、兴奋和公平。对随机性的需求推动了 [Chainlink 可验证随机函数](https://chain.link/chainlink-vrf)【VRF】的推出——这是一个专门为智能合约应用程序构建的随机数生成器，任何人都可以公开验证它是防篡改的。NFT 和区块链的游戏项目已广泛使用 Chainlink VRF 来选择赠品中的获胜者，在 NFT 造币期间分配特征，并以可证明的公平方式排序队列，以及其他用例。

与 DeFi 一样，大多数 NFT 和游戏应用程序需要被告知何时执行某些链上功能。例如，一个游戏可能需要一个链上交易来触发一轮游戏的开始，第二次调用 Chainlink VRF 获得一个随机数，第三次结束游戏并进行支付。应用程序无需花费时间和资源手动调用智能合同，而是可以将 Chainlink VRF 和自动化相结合，将整个流程外包出去，从而简化用户和开发人员的体验。

[【克拉巴达】](https://www.crabada.com/)，一个玩到赚的游戏，集成了 Chainlink VRF 和 Chainlink Automation，帮助提供可验证的随机、自动每周抽奖。VRF 帮助随机选择获奖者，**自动化节点**帮助开始和停止每轮抽奖，发起 VRF 呼叫，并向获奖者分发奖品。因此，Crabada 的每周幸运抽奖变得完全自动化，包括随机请求、奖励支付和连续游戏回合。

### 跨链互操作性+价格反馈

智能合同生态系统正日益向多链世界转变，在这一世界中，智能合同在许多区块链中广泛采用，每个都有自己的功能和应用。然而，区块链之间的互操作性仍然很原始，限制了令牌和命令跨环境无缝和安全桥接的能力。

[跨链互操作性协议](https://chain.link/cross-chain) (CCIP)是一个正在开发中的开源标准，旨在支持安全令牌桥和跨链消息传递路径。CCIP 的加入有可能从根本上改变 dApps 的多链战略。它可以利用 CCIP 成为一个单一的、互联的跨链应用，而不是在每个区块链上进行独立的部署。此外，通过与其他 Chainlink oracle 服务相结合，可以极大地增强 CCIP，例如价格馈送、可验证的随机性、自动化、[储备证明](https://chain.link/proof-of-reserve)、任何 API、公平排序服务等等。

例如，CCIP 和 Chainlink 价格馈送可以结合起来，为希望开通跨链抵押贷款的用户提供分散式货币市场应用。不稳定的抵押品可以存放在源区块链的智能合约中，允许在目的链上借入不同的资产，如稳定的债券。然后，Chainlink 价格馈送可用于计算贷款发放和清算期间的抵押率，而 CCIP 则跨链桥接抵押数据。因此，用户能够将他们的抵押品存储在高度分散的区块链上，同时在更高吞吐量的区块链或第二层网络上借入资产。

## 释放混合智能合同的全部潜力

与 Web 2.0 应用程序开发和 API 之间的关系类似，Web 3.0 dApp 开发也在以类似的模式前进，智能合同将关键服务外包给 oracle，并将 Oracle 服务组合在一起，以构建更复杂和易用的应用程序。这就是 Chainlink 致力于将 oracle 服务投入生产的原因，它是一种加速开发和释放新价值流的手段。

本文只是触及了使用组合式 Chainlink oracle 服务的可能性的皮毛。随着为混合智能合同开发框架带来广泛数据集和计算的其他 oracle 服务投入生产，可能性的范围只会加快。

如果您想现在就开始构建混合智能合约应用程序，并且需要某种类型的外部数据或计算，请参考我们的 [文档](https://docs.chain.link/docs/getting-started) ，在[Discord](https://discordapp.com/invite/aSK4zew)中询问一个技术问题，或者 [与我们的一位专家建立一次通话](https://chainlinkcommunity.typeform.com/to/OYQO67EF?page=homepage) 。

要了解更多，请访问[chain . link](https://chain.link/)，订阅 [Chainlink 简讯](https://chn.lk/newsletter) ，并关注 Chainlink 上的[Twitter](https://twitter.com/chainlink)，[YouTube](https://www.youtube.com/channel/UCnjkrlqaWEBSnKZQ71gdyFA)，以及[Reddit](https://www.reddit.com/r/Chainlink/)。