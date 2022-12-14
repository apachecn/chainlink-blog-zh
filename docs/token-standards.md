# 什么是令牌标准？

> 原文：<https://blog.chain.link/token-standards/>

令牌标准是一组公认的规则，用于指导给定区块链协议上的加密货币令牌的设计、开发、行为和操作。为了使令牌标准有用，它们必须被广泛采用。如果不被采纳，他们的规则就不能被提升到“标准”的地位——因为标准是广泛人群普遍遵循的规则。

在这篇博客中，我们将探讨为什么标准对增加加密令牌的采用、使用和价值如此重要。我们还将探索以太坊标准是如何发展的，并简要讨论 Solana 标准。

## 为什么我们需要令牌标准？

我们先来了解一下我们所说的“令牌”是什么意思。加密世界中的令牌通常是使用基于区块链的技术(最典型的是智能合约)创建、管理和分发的加密货币。代币可能具有市场价值，也可能具有某种效用，使得持有代币超出财务收益是可取的。

标准是一套“标准化”事物的规则。在令牌的上下文中，标准化意味着拥有一组规则，这些规则定义令牌应该包含什么数据、令牌能够执行的行为和动作，以及持有者或持有者群体可以对该令牌执行什么操作。令牌标准为基础区块链上令牌的创建、发行、部署、转移、销毁和其他属性提供了指导原则。正如所料，这些令牌标准很可能会出现在支持智能合约开发的区块链上，因为这样的区块链将能够支持在其上创建任意数量的令牌。

我们现在了解了什么是令牌标准，但要真正了解它们在生态系统中的作用，我们需要了解它们的好处。换句话说，我们为什么需要令牌标准，它们解决了什么问题？

一般来说，一个标准允许多种实现。例如，以太坊有客户端软件的标准，以太坊节点运行这些软件来提供与以太坊网络的连接。只要符合以太坊 [节点和客户端](https://ethereum.org/en/developers/docs/nodes-and-clients/) 的标准和规范，这些标准使得任何人都可以用他们选择的任何编程语言编写自己版本的以太坊客户端。因此以太坊网络可以有任意数量的节点和客户端，其中一些运行用 Golang、Rust、Java、C#、C++或 Python 编写的以太坊软件。这增加了"[](https://ethereum.org/en/developers/docs/nodes-and-clients/client-diversity/)"客户端多样性，通过减少对单个代码库实现的依赖，使网络更加强大。但是所有这些实现，不管软件语言、设计或实现细节如何，都有一个共同点——它们都遵循单一的客户规范。

因此，标准有助于增加实现的多样性。这意味着在安全性、速度、可伸缩性等方面可以有不同的方法。，这种多样性丰富了整体体验。这也意味着开发人员和设计人员可以参考一个稳定点来设计系统所需的最小行为集。这刺激了标准之上的创新，这释放了更多的用例，推动了更多的采用，等等。

这正是令牌标准如此重要的原因。标准化不同令牌的功能有助于开发人员在这些标准的基础上构建应用程序，因为他们知道只要应用标准，底层接口就会是相同的。

令牌标准的另一个巨大好处是实现它们的智能契约变得“可组合”这意味着我们可以设计契约来相互交互，因为标准化使我们能够知道符合标准的智能契约将公开什么功能、方法、数据类型和行为。然后，我们可以编写代码来与这些契约进行交互，这意味着智能契约的生态系统可以以不同的方式进行互操作、互连以及混合和匹配，以便整体远远大于部分的总和。一个简单的例子是，当一个新的加密令牌在基于以太坊的区块链网络上发行，并且它符合可替换令牌的 ERC-20 标准，那么它的智能合约将与设计用于 ERC-20 令牌的分散式交换兼容。

## 以太坊标准化流程

以以太坊为案例，我们可以对令牌标准的重要性有一个高层次的认识。以太坊遵循一个依赖于 [以太坊改进提案](https://eips.ethereum.org/) (EIPs)的流程，这些提案描述了适用于以太坊平台的标准(区块链的核心协议)、客户端和节点的 API，以及运行 [以太坊虚拟机](https://ethereum.org/en/developers/docs/evm/) 的智能合约。

有 [三种类型](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md#eip-types)EIP:

*   标准跟踪:这些通常包括核心协议的变更。它们有四个子类别:
    *   核心:以太坊协议的核心规则。
    *   联网:与网络协议规范的改进有关。
    *   接口:用于通过 RPC 与协议交互的 API 及相关变化，也包括 [智能合约 ABIs](https://blog.chain.link/what-are-abi-and-bytecode-in-solidity/) 等接口。
    *   ERC:以太坊请求评论；这些是应用程序的标准，包括智能合约和令牌标准。
*   元/进程 EIP:这些包括以太坊周围的进程，但不直接触及协议本身。这些建议可能会影响协议变更的决策过程等。
*   信息 EIP:这些包括以太坊设计问题或指南，社区将从中受益，但不改变或触及以太坊协议的特征。这些也是最可选的 EIP，因为用户可以随意忽略它们。

以太坊标准和指南通常有一个 [生命周期](https://eips.ethereum.org/) 用于 EIP 提案流程。这些生命周期阶段是“想法”、“草案”、“评审”、“最后一次呼叫”(最终评审)、“最终”(最终标准)、“停滞”(超过六个月不活动的非最终 EIP)、“撤销”和“活动”(正在运行但本质上是定期更新的 EIP，但从未真正实现最终目标)。

## 通用以太坊令牌标准

你可以在这里 找到所有 [的 EIP 列表。但是，让我们考虑一些流行的以太坊令牌标准。](https://eips.ethereum.org/)

正如我们在上一节中提到的，令牌标准是标准轨道的一部分，与应用于应用程序的规则相关，包括智能合约。

这其中最著名的很可能是 [ERC-20](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/) ，其中出了[EIP-20](https://eips.ethereum.org/EIPS/eip-20)。该标准适用于可替换令牌，即与另一个令牌在类型和值上完全相同的令牌，这意味着没有令牌是唯一的。这些是最常用的加密货币代币，可以在交易所交易。ERC-20 规定了一个令牌标准，它为智能合约中的令牌实现了一个 API。如果您点击上面的 ERC-20 相关链接，您可以看到 ERC-20 令牌智能合约必须实现的功能签名和事件列表，以符合该标准。这使得它们可以互操作，并使我们能够编写可以与这些令牌交互的应用程序，从而使生态系统更加可组合。开发人员还可以用他们喜欢的任何语言编写能够与这些 ERC-20 令牌智能合约交互的库，因为我们有一个 ERC-20 规范公开的所有功能和数据的列表。

Chainlink 的 [链接令牌契约](https://docs.chain.link/docs/link-token-contracts/) 也符合规范[ERC-677](https://github.com/ethereum/EIPs/issues/677)，该规范通过添加“transferAndCall”方法增强了 ERC-20 令牌的功能，该方法可被调用以触发令牌向接收方契约的转移，并调用接收方契约中的特定函数，所有这些都在单个事务中完成。这绕过了 ERC-20 的限制，即转让令牌是一个多步骤的过程，这增加了复杂性并增加了转让方的天然气费用。

另一个流行的令牌标准是 [ERC-721](https://ethereum.org/en/developers/docs/standards/tokens/erc-721/) 令牌标准涵盖了[EIP-721](https://eips.ethereum.org/EIPS/eip-721)。与 ERC-20 令牌不同，ERC-721 令牌允许向每个令牌添加某些独特的属性，这使得它们“不可替换”这个特性可以用来标记从艺术到个人身份的一切事物。由于该标准所要求的方法列表是众所周知的，像[Opensea](http://opensea.io)这样的 NFT 市场可以建立市场来列出、交易和销售非功能性交易，因为市场的代码可以与 NFT 的智能合约交互，以读取和写入与非功能性交易相关的数据。

由 [EIP 提出的对 ERC-721 的子改进-2309](https://eips.ethereum.org/EIPS/eip-2309) 标准化了创建或转移不可替换令牌时发出的事件的结构。

另一个流行的令牌标准是 [ERC-1155](https://ethereum.org/en/developers/docs/standards/tokens/erc-1155/) ，体现了[EIP-1155](https://eips.ethereum.org/EIPS/eip-1155)。这为可以发行 ERC-20 和 ERC-721 令牌类型的智能合约指定了标准。这被称为多令牌标准。这个 ERC 还试图纠正 ERC-20 和 ERC-721 的错误，并改善它们的功能。

在提案生命周期的各个阶段还有其他标准，您可以在闲暇时研究它们 [这里](https://eips.ethereum.org/) 。

## 索拉纳令牌标准

虽然令牌标准只是一种标准化规则(从化学品到建筑，我们都有！)，它们在不同的上下文中有不同的名称。在 [索拉纳](https://solana.com/) 生态系统中，令牌标准包含在索拉纳程序库(SPL)中，这是在索拉纳链的运行时上运行的链上软件程序的库。符合 SPL 标准的令牌与 Solana chain 和 Solana wallets 兼容，并增加了 Solana 生态系统中的可组合性。索拉纳令牌包含在索拉纳 [令牌计划](https://spl.solana.com/token) 中，该计划是整个 SPL 的一部分。这个程序为 Solana 兼容令牌的创建、发行、转移和销毁创建了一个标准化接口，可与以太坊生态系统中的 ERC-20 和 ERC-721 相媲美。

索拉纳的原生令牌是 SOL 令牌，堪比以太坊的 ETH。这也是 SPL 的象征。SPL 代币在功能上可以有很大不同，有些可以是非功能性代币，有些可以是可替换的，但发行量很小，还有一些可以结合各种其他类型的功能。

同样，遵守 SPL 标准意味着 Solana 上的令牌可以使用符合 SPL 标准的钱包和智能合约进行交互，从而在生态系统内实现可组合性和创新。它们还支持用于研究的分析和元信息，如可以在 [Solscan](https://solscan.io/tokens) 上看到的。该网站的功能由 SPL 标准启用，该标准规定了符合 SPL 标准的令牌必须具有的功能，以便可以构建这样的前端来与所有符合 SPL 标准的令牌进行交互。

## 结论

为了使运行在区块链网络上的智能合约有效运行，需要令牌标准来指导智能合约的实施，智能合约提供了可在其上构建分散式应用的编程接口。如果您是一名开发人员，现在正是创建自己的智能契约令牌[](https://blog.chain.link/how-to-create-an-erc-20-token-on-polygon/)的最佳时机，它实现了 ERC-20 令牌标准。您还可以通过学习这些标准如何帮助成长来加深您的技能 [DeFi](https://chain.link/education/defi) 协议、 [NFT 项目](https://chain.link/education/nfts) ，以及[](https://chain.link/education/metaverse)。你可以在这里 深入了解智能合约 [，如果你刚刚开始你的 Web3 开发者之旅，你可以在这里](https://chain.link/education/smart-contracts) 查看我们所有免费的 Web3 开发者技术教程 [。](http://blockchain.education)