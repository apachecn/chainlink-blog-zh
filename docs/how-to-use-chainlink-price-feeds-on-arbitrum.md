# 如何在 Arbitrum 上构建和部署智能合约

> 原文：<https://blog.chain.link/how-to-use-chainlink-price-feeds-on-arbitrum/>

Arbitrum 是一个以太坊第二层网络，支持开发人员以低成本构建和部署高度可扩展的智能合约。通过 Arbitrum 上的 Chainlink 数据馈送，开发人员可以快速轻松地将他们的智能合同连接到链外数据，包括可用于构建大量 DeFi 应用程序的高度可靠的资产价格。

在这篇技术文章中，我们将解释什么是 Arbitrum，介绍如何开始使用 Arbitrum Rinkeby Testnet，并逐步演示如何在 Arbitrum 智能合约中使用 Chainlink 价格馈送。虽然在本文中，我们将在 testnet 环境中构建和部署，但对于 Arbitrum One Mainnet，步骤是相同的。

[https://www.youtube.com/embed/wSq1YAkvqms?feature=oembed](https://www.youtube.com/embed/wSq1YAkvqms?feature=oembed)

## 什么是 Arbitrum？

Arbitrum 是一个 [乐观汇总](https://ethereum.org/en/developers/docs/scaling/layer-2-rollups/#optimistic-rollups) 以太坊第二层解决方案。一些扩展解决方案已经涌现出来，以在以太坊区块链上提供更高的速度和更低的成本，包括第二层汇总、状态(和支付)通道、侧链、等离子体和 Validium。这些解决方案之间最重要的区别是，汇总和状态通道继承了第 1 层以太坊区块链的安全性，使开发人员能够在基本以太坊层的基础上进行构建。

第二层汇总包括乐观汇总和 ZK 汇总。两者都是“真正的第二层解决方案”，这意味着它们能够以高速度和低成本执行大量交易，然后在第一层验证这一系列交易。通过乐观汇总，我们“乐观地相信”这些事务确实发生在第 2 层。这些汇总是“乐观的”,因为这些包被欺诈证据 视为“无辜的，直到被证明有罪”,并且被乐观地假设在发布到第 1 层时是正确的，除非在 7 天的质询期内提交了质询。

## Arbitrum 入门

在本教程中，我们将在 Arbitrum Rinkeby Testnet 上构建和部署一个智能合约，这是 Rinkeby Testnet 的第 2 层。同样的步骤也适用于以太坊主网的第二层 Arbitrum One。要使用 Arbitrum Rinkeby Testnet，我们需要一些 Rinkeby test ETH。你可以通过 [链接水龙头](https://faucets.chain.link/) 获得你的 Rinkeby 测试链接——只需粘贴你的钱包地址，选择 Rinkeby 以太坊，领取你的测试 ETH。

然后，我们需要从 Rinkeby 存放测试 ETH，以便在 Arbitrum Rinkeby Testnet 上支付费用。导航到 [公路桥](https://bridge.arbitrum.io/) ，连接您的钱包，输入您需要的 Rinkeby ETH 金额，点击“存款”。大约 10 分钟后，您会看到您的余额被记入第 2 层——是喝咖啡休息的时间了。

收到二层 ETH 后，将 Metamask 钱包配置为 Arbitrum Rinkeby testnet。或者导航到 [链表](https://chainlist.org/) 并找到 Arbitrum Rinkeby 网络的详细信息，或者导航到[Etherscan explorer](https://testnet.arbiscan.io/)并在网站页脚找到添加 Arbitrum 网络，或者选择设置- >网络- >在 Metamask 中添加网络，然后手动输入详细信息。

网络名称:Arbitrum Rinkeby Testnet

网络网址:https://rinkeby.arbitrum.io/rpc

链条 ID: 421611

货币符号:ETH

区块浏览器网址:https://testnet.arbiscan.io/

最后，返回 [链环龙头](https://faucets.chain.link/) ，选择 Arbitrum Rinkeby，权利要求 10 [测试链环令牌](https://testnet.arbiscan.io/token/0x615fbe6372676474d9e6933d310469c9b68e9726) 。

## 准确可靠的价格数据在智能合约中的重要性

为了扩展第 2 层智能合约的可能性，开发人员需要一个安全的连接到链外资源。利用 Chainlink oracles 提供的高度准确和可靠的价格数据，开发人员可以开始在 Arbitrum 上构建和测试各种可扩展的 DeFi 应用程序，这些应用程序依赖于 ETH 和其他令牌的价格，如借贷协议、分散式交易所、预测市场等。

虽然这些 DeFi 用例需要外部数据，但区块链和第 2 层解决方案无法从自身外部访问数据。当向区块链提供数据以促进高级 DeFi 用例时，为了防止 price oracle 攻击，数据的安全性和高质量是必不可少的。

Chainlink 价格馈送通过提供来自各种高质量数据提供商的聚合数据来减轻这些攻击的风险，这些数据由分散的 oracles 通过 Chainlink 网络在链上馈送。Chainlink 的分散 oracle 机制可确保最终价格值反映广泛的市场覆盖范围，这意味着最终价格是在汇总了整个市场的一组不同价格而不是一个小的子集后确定的，同时还考虑了交易量和流动性等其他方面。有了 Chainlink Price Feeds，开发人员能够构建不损害安全性的高级 DeFi 应用程序。

现在我们已经了解了在 Solidity smart contracts 中对准确可靠的价格数据的需求，以及 Chainlink 价格预测工具的重要作用，我们将通过一个示例，使用 Chainlink 价格馈送来获取 Arbitrum 上 Solidity smart contract 中 ETH 的最新价格。

## 使用 Arbitrum 上的链接价格馈送

从在你最喜欢的代码编辑器中创建一个新的 Solidity 项目开始。在 [Github](https://github.com/andrejrakic/chainlink-arbitrum) 上可以找到使用带打字稿的安全帽的完整示例。我们将使用最新版本的 Solidity 和 Chainlink。

```
import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
```

现在我们要编写一个函数来从 Chainlink 网络中检索价格馈送数据。导航到 Chainlink 官方文档的 [数据馈送部分](https://docs.chain.link/docs/get-the-latest-price/) 。我们将以“getPrice”函数为例，对其稍加修改。

```
 function getThePrice(address _priceFeedAddress) public view returns (int) {
        AggregatorV3Interface priceFeed = AggregatorV3Interface(_priceFeedAddress);

 (
 uint80 roundID,
 int price,
 uint startedAt,
 uint updatedAt,
 uint80 answeredInRound
        ) = priceFeed.latestRoundData();

 return price;
    }
```

您可以看到，我们将“priceFeedAddress”作为一个参数进行传递，以使这个智能契约更具可伸缩性。你可以在 Arbitrum Rinkeby Testnet[点击](https://docs.chain.link/docs/arbitrum-price-feeds/) 查看所有价格反馈地址的完整列表。

例如，如果我们想知道 BTC 的美元价格，我们可以将“0x0c 9973 e 7a 27d 00 e 656 B9 f 153348 da 46 CAD 70d 03d”作为参数“_ priceFeedAddress”传递给我们的函数。

## 快好了！认识 L2 音序器健康标志

Arbitrum 中的交易可快速确认。这是因为所谓的定序器。Sequencer 是一个链外组件，它能够高速订购用户事务并向用户提供收据。但是，如果 Sequencer 不可用，用户必须通过 Ethereum 提交他们的交易，以便在 Arbitrum 中处理它们。这对用户体验是有害的，许多 dApps 仍然没有能力同时与以太坊和 Arbitrum 交互。

如果您不想在应用程序中担心这个问题，您可以使用一个 Chainlink oracle 来确保序列器对用户可用。下面是这样做的步骤:

首先，我们需要将下一个导入语句添加到我们的实体代码

```
import "@chainlink/contracts/src/v0.8/interfaces/FlagsInterface.sol";
```

根据 Chainlink 文档，L2 测序仪健康标志由三个因素组成:

*   Chainlink Cluster(一组验证器节点)—它在每次心跳“T”(chain link feed 被配置为更新的最小频率)时执行 OCR 作业。
*   报告序列器状态的实际 OCR 输入——这可用于第 1 层的外部用户检查或协议(如 Arbitrum)状态。
*   验证器——由 OCR feed 触发，如果当前答案与前一个不同，则执行提高或降低标志动作。

现在，我们需要扩大我们与下一批线的合同

```
// Identifier of the Sequencer offline flag on the Flags contract 
address constant private FLAG_ARBITRUM_SEQ_OFFLINE = address(bytes20(bytes32(uint256(keccak256("chainlink.flags.arbitrum-seq-offline")) - 1)));
FlagsInterface internal chainlinkFlags;

constructor() {
 chainlinkFlags = FlagsInterface(0x491B1dDA0A8fa069bbC1125133A975BF4e85a91b);
}
```

“0x 491 B1 DDA 0a 8 fa 069 BBC 1125133 a 975 BF 4 e 85 a 91 b”是 Arbitrum Rinkeby 标志合同的地址。要查看其他地址，请到 [链接文档](https://docs.chain.link/docs/l2-sequencer-flag/) 。

上升的标志表示提要在“T”时间内没有更新，其数据可以被认为是陈旧的。换句话说，定序器离线了，您的合同不应该执行任何关键操作。当定序器再次恢复运行并且第二层链接数据更新时，您可以像往常一样继续使用您的合同。让我们增加额外的检查。

```
 function getThePrice(address _priceFeedAddress) public view returns (int) {
 bool isRaised = chainlinkFlags.getFlag(FLAG_ARBITRUM_SEQ_OFFLINE);
 if (isRaised) {
 // If flag is raised we shouldn't perform any critical operations
 revert("Chainlink feeds are not being updated");
        }

 AggregatorV3Interface priceFeed = AggregatorV3Interface(_priceFeedAddress);
 (
 uint80 roundID,
 int price,
 uint startedAt,
 uint updatedAt,
 uint80 answeredInRound
 ) = priceFeed.latestRoundData();
 return price;
    }
```

## 部署和测试智能合约

现在我们已经准备好部署和测试我们的合同。在 [Remix](https://remix.ethereum.org/) 中编译契约，然后在 deployment 选项卡上，将环境更改为“Injected Web3”。确保钱夹连接到 Arbitrum Rinkeby Testnet，并且下面的钱夹地址用于包含先前获得的 ETH 的 MetaMask 钱夹。然后，按下 deploy 按钮并按照步骤操作。

最终结果是成功的交易和部署到 Arbitrum Rinkeby Testnet 的智能合约。

为了测试它，我们只需要调用我们的“getThePrice”函数，并在 Arbitrum Rinkeby Testnet 上传递一个链接价格馈送地址作为“_ priceFeedAddress”参数。请记住，您可以在[chain link Docs](https://docs.chain.link/docs/arbitrum-price-feeds/)中看到所有可用的价格馈送地址。

## 摘要

Chainlink 广受欢迎的 ETH/USD 价格馈送以及 LINK/USD、AAVE/USD 和 BTC/USD 的价格馈送都可以在 Arbitrum 上获得。这些 Chainlink 价格馈送建立在分散式 oracle 基础架构之上，该基础架构由众多经过安全审查的节点操作符和优质数据源组成，可提供高度准确、可用且防篡改的数据馈送，这些数据馈送本身就能抵御闪贷引发的价格操纵攻击等漏洞。

本技术教程展示了在 Arbitrum 上编写和部署混合智能合约是多么容易。有了这些知识，您将能够开始构建自己的智能合约，利用 Arbitrum 的低成本和高速度以及 Chainlink Price Feeds 解锁的高级实用程序。

访问[chain . link](https://chain.link/)或阅读[docs . chain . link](https://docs.chain.link/)了解更多关于 Chainlink 的信息。要讨论整合，[联系专家](https://chainlinkcommunity.typeform.com/to/OYQO67EF)。