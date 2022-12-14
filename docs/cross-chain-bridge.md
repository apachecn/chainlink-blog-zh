# 什么是跨链桥？

> 原文：<https://blog.chain.link/cross-chain-bridge/>

web 3 生态系统正变得越来越多链，分散的应用程序存在于 [数百个不同的区块链](https://defillama.com/chains) 和第二层解决方案中，每个解决方案都有自己的安全和信任方法。由于 [区块链可扩展性](https://blog.chain.link/blockchain-scalability-approaches/) 的持续挑战，这一趋势很可能会持续下去，这将得到更多区块链、第 2 层和第 3 层解决方案以及独立网络(如专用区块链)的推出的支持，这些网络可以根据单个或更小的分散应用集群的特定技术和经济要求进行定制。

然而，区块链人天生就不能互相交流。这使得 [【区块链】互操作性](https://blog.chain.link/blockchain-interoperability/) 对于实现多链生态系统的全部潜力至关重要。区块链互操作性的基础是跨链消息协议，它使智能合约能够从其他区块链读取数据和向其写入数据。

随着如此多的经济活动被孤立在孤立的网络上，越来越明显的是，Web3 需要强大的跨链互操作性解决方案，使数据和令牌能够以安全和无缝的方式在区块链的互联网络上移动。

作为跨链互操作性的核心元素，跨链桥是一种基础设施，它使令牌能够从源区块链传输到目的地区块链。

本文解释了什么是跨链桥以及跨链桥的不同类型，探讨了与跨链桥相关的设计挑战，并研究了即将推出的 [跨链互操作协议](https://chain.link/cross-chain)【CCIP】如何解决这些限制。

## 为什么 Web3 中需要交叉链桥

[](https://blog.chain.link/what-is-blockchain/)它们本身并不能相互通信——它们通常没有能力监控或了解其他网络上发生的事情。当涉及到协议设计、货币、编程语言、治理结构、文化和其他元素时，每个链都有自己的一套规则，这使得链之间的通信很困难。这种区块链内部交流的缺乏限制了在 [Web3](https://chain.link/education/web3) 生态系统中可能发生的经济活动的数量——如果没有区块链互操作性，不同的网络实际上代表着完全不同的孤立经济体，它们之间没有连通性。

理解跨链桥的必要性的一个简单方法是把区块链想象成不同的大陆，它们之间有广阔的海洋。A 大陆拥有丰富的自然资源，B 大陆拥有种植粮食的肥沃土地，C 大陆拥有蓬勃发展的制造业和大量熟练技工。

如果我们能够将这些大陆的能力结合起来，我们将会拥有一个繁荣的世界。但是，如果没有办法通过航运、桥梁、隧道或其他基础设施将不同的经济体连接起来，这些地区将无法从自身的能力中获益。A 大陆将没有食物，B 大陆将没有技术来最大限度地提高其食物生产效率，C 大陆将没有资源来制造最好的产品。然而，如果我们能够连接这些经济体，所有大陆都将受益于一个更加互联互通的世界，每个地区都可以专注于自己独特的能力，同时通过贸易受益于整个世界的财富和创造力。

同样，通过支持不同的区块链、扩展解决方案和特定于应用的链进行通信，生态系统可以从每个区块链生态系统的个体质量中受益。

## 跨链桥是如何工作的？

跨链桥是一种分散式应用程序，能够将资产从一个区块链转移到另一个。跨链网桥通过促进不同区块链之间的跨链流动性来增加令牌效用。跨链桥通常涉及通过 [智能契约](https://chain.link/education/smart-contracts) 锁定或烧毁源链上的令牌，并通过目的链上的另一个智能契约解锁或铸造令牌。

令牌桥通常利用跨链消息协议来实现特定目的——在区块链之间移动令牌。实际上，跨链桥是跨链消息传递协议的一个非常狭窄的用例，许多桥只是作为两个区块链之间特定于应用程序的服务。在其他情况下，跨链桥用于促进更广泛的效用，例如跨链[【dex】](https://blog.chain.link/dex-decentralized-exchange/)、跨链 [货币市场](https://blog.chain.link/decentralized-money-markets/) ，或者更提供更一般化的跨链功能。

## 交叉链桥的类型

交叉链桥由三种主要机制类型驱动:

*   **锁定并铸造**—用户在源链上的智能合约中锁定令牌，然后在目标链上将这些锁定令牌的包装版本铸造为一种形式的 IOU。反方向，目的链上包裹好的代币被烧掉，解锁源链上的原币。
*   **刻录和铸造**—用户在源链上刻录令牌，然后在目标链上重新颁发(铸造)相同的本地令牌。
*   **锁定和解锁**—用户锁定源链上的令牌，然后从目标链上的流动性池中解锁相同的本地令牌。这些类型的跨链桥梁通常通过收入共享等经济激励措施吸引桥梁两侧的流动性。

此外，跨链网桥还可以与任意数据消息传递功能相结合，这种功能不仅可以在区块链之间移动令牌，还可以移动任何类型的数据。这些 **可编程令牌桥** 涉及令牌桥接和任意消息传递的组合，一旦令牌被递送到目的地链，就在目的地链上执行智能合约调用。

在完成桥功能后，可编程令牌桥能够实现更复杂的跨链功能。这些包括在执行桥接功能的同一交易中，在目的地链上交换、借出、押入或存放智能合约中的令牌。

对跨链桥进行分类的另一种方法是检查它们在 [信任最小化](https://blog.chain.link/what-is-trust-minimization/) 光谱中的位置，当涉及到验证源区块链的状态并将随后的事务中继到目的地区块链时。通常情况下，跨链解决方案越接近信任最小化，计算成本就越高，灵活性和可推广性就越差。这些权衡是为了支持需要最强信任最小化保证的用例。

## 跨链桥接的挑战

没有可信第三方的区块链之间的安全通信是一项挑战。跨链通信本质上需要安全性、信任或灵活性的折衷，而在单个区块链上进行的交互并不需要这些。这也意味着，不同区块链上的智能合约之间的[](https://blog.chain.link/defis-permissionless-composability-is-supercharging-innovation/)的可组合性只能通过在安全性、信任假设或配置灵活性方面进行内在的权衡来实现，而这对于同一链上的智能合约之间的可组合性是不需要的。

如果跨链消息传递存在限制，为什么不将所有应用程序活动部署在单个区块链上呢？这个问题的答案是双重的。首先，如果去中心化和可信的中立性是网络的核心价值，那么由于计算能力、带宽和存储能力，单个区块链可以处理多少活动存在 [理论限制](https://vitalik.ca/general/2021/05/23/scaling.html) 。其次，单个区块链和扩展解决方案针对不同的质量进行优化，例如速度、安全性和分散性，并且很可能在这些价值的最佳组合方面总是存在分歧，从而导致对多个链和解决方案的需求。

与跨链桥相关的一个重要问题是包装资产与本地资产的使用。包装或桥接资产是源区块链上另一个资产的表示，因此，由于需要一个或多个实体保管底层令牌，因此引入了各种形式的安全和信任假设。这些限制可以通过由 [链节储备证明](https://chain.link/proof-of-reserve) 支持的分散验证来减轻。如果使用本地资产，那么在执行桥接功能之后，实际上在目的链上使用相同的资产，尽管需要考虑如何验证一个链上的令牌的燃烧以触发另一个链上的发布。

跨链桥的另一个考虑因素是终结性——目的链上的资金一旦在源链上成功提交，就可以得到保证。如果没有保证终结性，源链上的反向事务(如块重组)可能会对目标链产生不利影响，如创建无支持的桥接令牌。

加密经济系统的弹性取决于其最弱的攻击载体。即使底层的区块链或第二层网络是安全的，不安全的网桥也会使资金变得脆弱。保护桥梁的关键考虑因素是攻击的成本和需要贿赂的参与者的数量。在这个意义上，最大化跨链桥的安全性意味着最大化实体的多样性和/或加密保证的强度，以在状态验证和后续交易向目的地区块链的中继期间保护桥。

由于这些复杂性，对网桥的攻击占据了 Web3 领域中 [漏洞利用](https://www.theblock.co/data/decentralized-finance/exploits) 的很大一部分，这就要求在设计跨链消息协议时必须有安全第一的思想。

## 如何构建 CCIP 以实现安全的跨链桥接

为了适应区块链生态系统中对安全跨链消息传递不断增长的需求，Chainlink 正在开发 [【跨链互操作性协议(CCIP)](https://chain.link/cross-chain)——一种用于跨链通信的开源标准，包括任意消息传递和跨链令牌传输。CCIP 的目标是通过一个单一的标准化接口在数百个区块链网络之间建立一个通用连接。此外，CCIP 正在构建中，因此它可以与各种其他 oracle 服务组合，以支持高度复杂的跨链交互和 [跨链智能合约](https://blog.chain.link/cross-chain-smart-contracts/) 。

预计 CCIP 将支持多种跨链服务，包括可编程令牌桥，这将使用户能够以高度安全、可扩展和经济高效的方式在任何区块链网络中移动令牌。可编程令牌桥旨在为开发人员提供支持计算的服务，以便在区块链网络中安全传输令牌，并在目标链上启动涉及使用这些桥接令牌的编程操作。

通用接口能够将令牌转移到 EVM 和非 EVM 连锁店的任何 Chainlink 集成区块链网络，不仅可以释放不同区块链网络之间的闲置流动性，还有助于提高跨连锁店信息传递的安全标准。通过反欺诈网络提高安全性，反欺诈网络是一个独立的网络，用于监控恶意活动；来自各种高质量节点运算符的分散式 oracle 计算，具有可验证的链上性能历史记录；以及离线报告(OCR 2.0)协议，这是 OCR 1.0 的一个更通用的实现，已经促成了数万亿美元的总价值(TVE)。

要了解更多关于 CCIP 的信息，以实现更强大的跨链生态系统，请阅读本博客: [介绍跨链互操作性协议(CCIP)](https://blog.chain.link/introducing-the-cross-chain-interoperability-protocol-ccip/) 。

要了解更多关于 Chainlink 的信息，请访问 [Chainlink 网站](https://chain.link/) 并关注官方[Chainlink Twitter](https://twitter.com/chainlink)以了解最新的 chain link 新闻和公告。