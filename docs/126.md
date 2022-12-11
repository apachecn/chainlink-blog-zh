# 借助安全的数据馈送，在金融智能链上构建 dapp

> 原文：<https://blog.chain.link/build-a-dapp-on-binance-smart-chain-with-secure-data-feeds/>

*chain link 2021 秋季黑客马拉松于 10 月 22 日拉开帷幕。* [*今天报名。*](https://chain.link/hackathon?utm_medium=referral&utm_source=chainlink-blog&utm_campaign=fall-2021-hackathon&utm_content=build-a-dapp-on-binance-smart-chain-with-secure-data-feeds)

2019 年 4 月，受欢迎的加密货币交易所币安推出了[币安链](https://academy.binance.com/en/articles/an-introduction-to-binance-smart-chain-bsc)，这是一个为快速交换而构建和优化的网络，也是 BNB 令牌的家园。在币安链条上，您可以:

*   发送和接收 BNB
*   发行新代币
*   发送、接收、刻录/铸造和冻结/解冻令牌
*   建议在两个不同的代币之间创建交易对
*   通过在链上创建的交易对发送购买或出售资产的订单

币安链是伟大的用户寻求加快他们的交易。然而，它与 EVM 不兼容，也不支持智能合约，这是由设计决定的。为了允许智能合同的创建，币安团队还创建了[币安智能链](https://www.binance.com/en/blog/421499824684900933/Binance-Smart-Chain-Launches-Today) (BSC)，这是一个使用 PoSA 共识算法的网络，与 EVM 兼容，使智能合同开发者能够构建可编程的 dApps，与币安链进行本地集成。

这种双链架构允许在交易所端进行快速交易，同时仍然支持智能合约。币安团队实际上创造了术语“CeDeFi”，或“集中分散金融”，来描述这种混合方法，这种方法在使用较少分散的架构来增加交易吞吐量的同时，仍然确保开发人员可以以无许可的方式部署应用程序，并使用以太坊的相同可组合工具，如 Solidity 和 [Chainlink Price Feeds。](https://chain.link/solutions/defi)

[币安智能链 Chainlink 价格反馈](https://docs.chain.link/docs/binance-smart-chain-addresses)可在币安 mainnet 上获得，可用于在 BSC 上构建需要分散、防篡改数据输入的应用。在本技术教程中，我们将带您了解如何使用币安智能链、BNB 令牌和 Chainlink [甲骨文](https://chain.link/education/blockchain-oracles)，以便您可以快速开始在 BSC 上构建外部连接的[智能合约](https://chain.link/education/smart-contracts)，即使您没有以太坊的经验。

# 使用 BSC 构建

## 我们将使用的工具

由于 BSC 是 EVM 兼容的，我们可以在我们的 Solidity 环境中使用相同的工具，比如 [Truffle](https://www.trufflesuite.com/) 、 [Hardhat](https://hardhat.org/) 、 [MetaMask](https://metamask.io/) 等等。在本教程中，我们将使用 [Brownie](https://github.com/eth-brownie/brownie) ，一个 Python 智能契约开发框架，来与 BSC 一起工作，因为 Brownie 有一个强大的分叉特性，我们可以在本地使用，因为币安测试网目前不支持价格馈送。

如果你有兴趣用币安智能链而不是布朗尼运行松露或安全帽程序，请查看 [ganache-cli 的分叉功能](https://github.com/trufflesuite/ganache-cli)或跳到我们讨论分叉的地方——我们将提到如何用 ganache 运行它。在运行我们的测试时，Brownie 在后端使用 ganache-cli 进行分叉。

## 要求

*   python3
*   nodejs
*   加纳切-cli
*   6-8 岁的女童子军

要检查 Python 版本，请在终端中键入:

```
python --version

```

对于 nodejs，键入:

```
node -v

```

你可以在这里下载 [python](https://www.python.org/downloads/) 和 [nodejs](https://nodejs.org/en/) 。节点预装了 *npm* 。然后，确保您的 *ganache-cli* 安装了:

```
npm install -g ganache-cli

```

或者

```
yarn global add ganache-cli

```

最后，安装布朗尼与

```
pip install eth-brownie

```

或者

```
pip3 install eth-brownie

```

现在我们都准备好了！

## 入门指南

现在我们已经设置好了一切，继续打开布朗尼的 [chainlink-mix。](https://github.com/smartcontractkit/chainlink-mix)这是使用 Chainlink 智能合同的样板模板。如果你想了解更多，你可以在我们的博客中学习如何[用 python 部署任何区块链的智能合约。](https://blog.chain.link/develop-python-defi-project/)

要使用 mainnet 或 testnet 币安智能链，您通常需要 BNB 令牌，类似于在以太坊区块链上使用 ETH。当您部署智能合同时，您需要将它们与 BNB 一起部署。

我们将 100%在本地做所有事情，所以我们不需要任何 testnet ETH、LINK 或 BNB 来开始。

首先，让我们烘烤布朗尼混合蛋糕:

```
brownie bake chainlink-mix
cd chainlink

```

我们在我们的项目中。如果我们运行 *ls* ，我们可以看到我们的目录中有什么。

*   构建:这是项目跟踪您部署的智能契约和编译的契约的地方
*   合同:合同的源代码，通常用 Solidity 或 Vyper 编写
*   接口:您将需要使用部署的契约的接口布局。每次与合同的交互都需要一个 [ABI](https://ethereum.stackexchange.com/questions/234/what-is-an-abi-and-why-is-it-needed-to-interact-with-contracts) 和一个地址。接口是获得合同 ABI 的好方法
*   脚本:我们创建的脚本，用于自动化处理我们的合同
*   测试:测试
*   brownie-config.yaml:这是 brownie 了解如何使用智能合约的所有信息。我们想部署到什么样的区块链？有什么我们想设置的特殊参数吗？所有这些都在配置文件中设置。

*requirements.txt* 、 *README.md* 、 *LICENSE* 和*。gitignore* 暂时可以忽略。当你练习的时候，你会发现它们是干什么用的。

我们将把 *PriceFeed.sol* 部署到我们的本地环境中，这将从币安主链分支。

## 添加工作网络

为了使用币安链，我们需要一个[远程过程调用(RPC)](https://en.wikipedia.org/wiki/Remote_procedure_call) URL 或者*主机*。这是一个 URL，我们将调用 API 来连接币安智能链。如果有兴趣，你也可以[运行你自己的币安智能链节点](https://docs.binance.org/smart-chain/developer/fullnode.html)并与之连接。

我们可以在他们的文档中找到[币安的 RPC URLs。我们还需要链号。现在，让我们使用这些:](https://docs.binance.org/smart-chain/developer/rpc.html)

```
host=https://bsc-dataseed.binance.org/ chainid=56 

```

最后，我们需要我们想要使用的链接价格源的价格源地址。布朗尼 chainlink-mix 预装了币安 ETH/USD 价格馈送，但是如果你想要一份币安所有 chainlink 价格馈送的列表，你可以随时查看币安或 [Chainlink 文档](https://docs.chain.link/docs/binance-smart-chain-addresses)了解更多信息。如果你在 *brownie-config.yaml* 文件中查找，你会在*网络*下看到一个叫做*币安分叉*的部分。这是我们使用币安叉子时需要的所有变量。

现在，我们想告诉布朗尼使用币安连接，但我们想叉它。分叉一个链意味着获取该链的一个副本并在本地运行它，这样我们就不必支付任何费用，并且可以快速迭代测试。这也意味着，一旦分叉链关闭，所有内容都将被删除！我们可以使用以下命令将币安的一个分支添加到布朗尼网络:

```
brownie networks add development binance-fork cmd=ganache-cli host=http://127.0.0.1 fork=https://bsc-dataseed1.binance.org accounts=10 mnemonic=brownie port=8545

```

这将在本地主机的端口 8545 上运行一个本地 *ganache-cli* 链。当我们部署时，它将使用 https://bsc-dataseed1.binance.org 的*作为它的来源分叉。如果操作正确，您将看到如下内容:*

```
Brownie v1.13.0 - Python development framework for Ethereum

SUCCESS: A new network 'binance-fork' has been added
  └─binance-fork
    ├─id: binance-fork
    ├─cmd: ganache-cli
    ├─cmd_settings: {'fork': 'https://bsc-dataseed1.binance.org', 'accounts': 10, 'mnemonic': 'brownie', 'port': 8545}
    └─host: http://127.0.0.1

```

您可以通过运行*布朗尼网络列表*来检查所有网络。

## 部署您的合同

现在一切都设置好了，我们可以在本地 ganache 分叉链上部署和读取我们的契约。在[脚本](https://github.com/smartcontractkit/chainlink-mix/tree/master/scripts/price_feed_scripts)文件夹中有一个名为[deploy _ price _ consumer _ v3 . py](https://github.com/smartcontractkit/chainlink-mix/blob/master/scripts/price_feed_scripts/deploy_price_consumer_v3.py)的脚本。我们可以这样运行:

```
brownie run scripts/price_feed_scripts/deploy_price_consumer_v3.py  --network binance-fork

```

您将看到如下输出:

```
Brownie v1.13.0 - Python development framework for Ethereum

ChainlinkMixProject is the active project.

Launching 'ganache-cli --accounts 10 --fork https://bsc-dataseed1.binance.org --mnemonic brownie --port 8545 --hardfork istanbul'...

Running 'scripts/price_feed_scripts/deploy_price_consumer_v3.py::main'...

Transaction sent: 0x63022ee6c741ffb31ec6f8f29d3d2412c0a81a557a316a9a9752603825b8e96d
  Gas price: 0.0 gwei   Gas limit: 6721975   Nonce: 0
  PriceFeed.constructor confirmed - Block: 4398765   Gas used: 132364 (1.97%)
  PriceFeed deployed at: 0x3194cBDC3dbcd3E11a07892e7bA5c3394048Cc87
The current price of ETH is 135462000000
Terminating local RPC client...

```

所以，我们只是:

1.  分叉财务链并在本地运行
2.  为 it 部门部署智能合同
3.  从中读出以太的价格

```
The current price of ETH is 135462000000

```

恭喜你！你正在申请币安奖金的路上！

**更进一步**

现在，您已经知道如何使用币安智能链部署智能合约，您可以更深入地了解 BSC，甚至使用 Matic、xDai 或其他侧链和第 2 层。如果您更喜欢 Hardhat 和 Truffle，看看您是否可以在那里使用 *ganache-cli* 命令并运行一些本地测试。即将举办许多黑客马拉松，所以一定要参加，以便有机会与该领域的其他有才华的人一起工作，赢得一些奖品，并成为一名聪明的合同开发人员。

如果您想继续扩展您的智能合约的功能，请访问 Chainlink [开发者文档](https://docs.chain.link/)并加入 [Discord](https://discordapp.com/invite/aSK4zew) 的技术讨论。如果您使用币安智能链、布朗尼、松露、安全帽或任何其他 Chainlink 集成构建了一些很棒的东西，请务必用@chainlink 标记我们，这样我们就可以查看您所做的所有很酷的工作！

*[chain link 2021 秋季黑客马拉松](https://chain.link/hackathon) 于 2021 年 10 月 22 日开赛。无论您是开发人员、创作人员、艺术家、区块链专家，还是该领域的新手，这个黑客马拉松都是启动您的智能合同开发之旅并向行业领先的导师学习的最佳场所。立即锁定您的席位，争夺超过$ 300，000 的奖金。T9】*

[Sign up today](https://chain.link/hackathon?utm_medium=referral&utm_source=chainlink-blog&utm_campaign=fall-2021-hackathon&utm_content=build-a-dapp-on-binance-smart-chain-with-secure-data-feeds)