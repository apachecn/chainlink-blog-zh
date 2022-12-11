# 使用 chainlink oracle 安全利用曲线 lp 池

> 原文：<https://blog.chain.link/using-chainlink-oracles-to-securely-utilize-curve-lp-pools/>

DeFi 协议在它们保护的链上价值的数量上正在增长，并且作为结果，恶意的对手在经济上更有动机利用任何潜在的攻击媒介。正如我们在今年早些时候的博客文章[中警告的那样，DeFi 智能合约的数据质量的重要性](https://blog.chain.link/the-importance-of-data-quality-for-defi/)，DeFi 中最大的漏洞是依赖于数据质量差的价格预言的协议，如 AMM dex 生成的链上价格预言。由于其较低的交易量和/或缺乏市场覆盖，这些单一来源的价格先知越来越多地被闪贷操纵，DeFi 协议依赖它们获得其[智能合约](https://chain.link/education/smart-contracts)，最终导致用户资金损失。

虽然总部位于 AMM 的 dex 作为即时获得流动性的交易环境为该领域带来了巨大价值，但它们绝对不是一个可靠的甲骨文机制，无法为用户确保数百万至数十亿美元的安全。资产管理公司的本质创造了资产池中持有的资产储备价值严重扭曲的时刻，从而产生了最近被闪贷攻击利用的套利机会。

虽然 Curve 是一种基于 AMM 的有价值的指数，但最近的闪贷漏洞进一步证明，它不应被其他 DeFi 协议用作价格预测工具，即使在用作抵押品时，也不应根据其他链上资产对 Curve LP 令牌进行定价。相反，我们鼓励所有需要为稳定币或加密货币中的曲线 LP 令牌定价的 DeFi 协议使用 [Chainlink Price Feeds](https://chain.link/solutions/defi) ，这是专为避免这些攻击媒介而构建的，许多 DeFi 应用程序使用 Chainlink Price Feeds 来确保数十亿美元的安全并保持不受影响。

我们已经在 mainnet 上直播了以下价格馈送:[和/或/和](https://feeds.chain.link/usdc-eth)、[、【TUSD 和/或/和](https://feeds.chain.link/tusd-eth)、[、](https://feeds.chain.link/usdt-eth)和[戴和/或/和/或/和](https://feeds.chain.link/dai-eth)，可以用来计算曲线 LP 代币的价值。这些价格馈送会根据价格波动不断更新，直接存储在链上，并且可以在一次调用中轻松引用，以获得曲线 LP 令牌的可信价格。

为了理解为什么 Chainlink Price Feeds 是使用 Curve LP 令牌的 [DeFi](https://chain.link/education/defi) 协议的最佳解决方案，让我们快速检查一下我们为防止 flash loan 攻击而引入的特定功能，并展示开发人员如何快速将这些分散的价格 oracles 集成到他们今天的应用程序中。

## **闪贷攻击漏洞以及 Chainlink 如何防范这些漏洞**

Curve Finance 的混合[自动做市商(AMM)](https://blog.chain.link/challenges-in-defi-how-to-bring-more-capital-and-less-risk-to-automated-market-maker-dexs/) 旨在使用双边链上流动性池，在类似资产(如稳定债券)之间提供低滑动掉期。虽然稳定货币之间的汇率相对稳定，但当大量交易在曲线上进行时，相对于这些资产的更广泛市场价格，价格可能会暂时扭曲。然后，这种价格扭曲可以被用来利用 DeFi 协议，该协议提供 Curve LP 令牌作为抵押品，并依赖 Curve 本身作为 oracle 来为这些令牌定价。

鉴于 DeFi 的无许可性质和多个快速贷款提供商的崛起，获得实施此类攻击所需资本的障碍要低得多。随着曲线 LP 令牌越来越多地在 DeFi 协议中得到利用，这些项目使用具有广泛市场覆盖范围的 Chainlink 等 oracles 来保护其用户免受通过快速贷款和/或在曲线协议上进行的大额交易造成的暂时价格扭曲，这一点至关重要。

每个 Chainlink 价格馈送都由领先的安全和区块链 DevOps 团队运营的抗 Sybil Oracle 节点运营商分散网络保护。oracle nodes 从多个链外数据聚合器获取价格数据，这意味着每个价格点都具有反映所有交易环境的强大的量调整市场覆盖范围。然后将所有节点的响应汇总起来，形成一个单一的价格更新，进一步增强了数据的可用性和对来自任何一个节点或数据源的操纵的抵抗力。

重要的是，Chainlink 还保持着强大的经济激励，在像 Infura 这样的网络中断期间保持 oracle networks 正常运行，即使在油价高企的动荡市场条件下也公布新价格。这一切都可以由用户通过[链上可视化](https://feeds.chain.link/)和[节点列表服务](https://market.link/)来验证，这些服务允许用户监控 oracle 网络和单个节点运营商的实时和历史性能。

由于 Curve 仍然是稳定货币流动性的广泛使用的 AMM，我们鼓励所有 DeFi 协议使用 Chainlink Price Feed oracles 作为对其用户持有的 Curve LP 令牌进行定价的机制。扩展 DeFi 的一个关键组成部分是扩展支撑它的安全性，尤其是最终触发智能合约结果的 oracles。我们将继续致力于提供市场上最安全、最可靠的价格预测工具，并期待通过让 DeFi 协议访问持续准确的链上估价(可抵御数据操纵攻击)来促进曲线 LP 令牌的使用。

## 如何为 LP 代币建立抗闪贷的估值机制

如果你是一个在你的 DeFi 协议中寻找定价曲线 LP 令牌的建筑商，这里有一个关于如何整合 Chainlink 价格馈送以确保公平市场估值的简短解释。

**1。为您的 LP 令牌找到正确的价格源**

首先，您需要为每个 LP 令牌确定合适的链接价格提要。例如，如果您想要为 ETH 中的 [3pool](https://www.curve.fi/3pool) 定价，您应该为池中的每个令牌使用各自的提要，在本例中为戴、和。因此，您将开始使用[戴/ETH](https://feeds.chain.link/dai-eth) 、 [USDT/ETH](https://feeds.chain.link/usdt-eth) 和 [USDC/ETH](https://feeds.chain.link/usdc-eth) 价格源。

**2。获取您的合同的报价地址**

您可以在这里找到每个价格源及其各自的地址[。我们还鼓励开发人员在](https://docs.chain.link/docs/ethereum-addresses) [【电子邮件保护】](/cdn-cgi/l/email-protection#fc8f898c8c938e88bc9f949d9592d290959297) 联系我们的整合团队，以便深入了解价格反馈机制以及利用它们的最佳方式。

**3。查询每个 feed 的最新价格，取最低价格**

一旦你找到了正确的地址，你就可以从链接文档的[部分](https://docs.chain.link/docs/get-the-latest-price)中得到每一个 feed 的价格。查询完价格后，取其中的最小值，用 *min_value = min (price1，price2 … priceN)* 表示

**4。获取 LP 令牌的虚拟价格，并将其乘以之前获得的最小值**

曲线中的虚拟价格是通过获取库的不变性而获得的，默认情况下，库将每一个稳定的币都视为 1.00 美元。可以通过为其调用 *get_virtual_price* 函数来获得每个池的虚拟价格。一旦你得到了这个价格，你现在可以用之前得到的 *min_value* 乘以它。因此，*最小价格=最小价值*虚拟价格。*最后，当您的合约调用 *get_virtual_price* 时，添加[重入](https://solidity-by-example.org/hacks/re-entrancy/)检查，因为曲线虚拟价格[容易受到](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/)这样的攻击。

现在，您可以使用我们刚刚计算的价格作为您的应用程序中 LP 令牌价值的下限，这既可靠又能抵御任何类型的 flash loan 攻击。如对此实施有任何疑问，请[联系我们](/cdn-cgi/l/email-protection#92e1e7e2e2fde0e6d2f1faf3fbfcbcfefbfcf9)，我们很乐意帮助您将这一可靠的定价机制整合到您的应用中。

要了解更多信息，请访问 [chain.link](https://chain.link/) ，订阅 [Chainlink 简讯](https://chn.lk/newsletter)，并在 Twitter 上关注 [@chainlink](http://www.twitter.com/chainlink) 。

[Docs](https://docs.chain.link/docs/getting-started)|[Discord](https://discordapp.com/invite/aSK4zew)|[Reddit](https://www.reddit.com/r/Chainlink/)|[YouTube](https://www.youtube.com/channel/UCnjkrlqaWEBSnKZQ71gdyFA)|[Telegram](https://t.me/chainlinkofficial)|[Events](https://blog.chain.link/tag/events/)|[GitHub](https://github.com/smartcontractkit/chainlink)|[Price Feeds](https://feeds.chain.link/)|[DeFi](https://www.chain.link/solutions/defi)|[VRF](https://chain.link/solutions/chainlink-vrf)