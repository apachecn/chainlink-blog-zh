# Solidity 中的 ABI 和字节码是什么？

> 原文：<https://blog.chain.link/what-are-abi-and-bytecode-in-solidity/>

当你作为一名 Solidity 开发人员开始你的旅程，并开始积极地编写以太坊 [智能契约](https://chain.link/education/smart-contracts) 时，你会很快遇到对 EVM(以太坊虚拟机)、字节码和 ABI(应用程序二进制接口)的引用。如果您是一名 JavaScript 开发人员(当我第一次学习编码时也是如此)，这些术语可能对您并不熟悉，或者您可能在其他上下文中听说过它们，并且想知道它们在 Solidity 和 Ethereum 世界中的意思是否相同。

本博客提供了这三个概念的技术概述。与代码中的所有事情一样，很容易迷失在这些概念的兔子洞里，这可能会适得其反。相反，这个博客将为你提供每个概念的坚实的心智模型，这样你就有了立即变得富有成效所需的一切。

到本博客结束时，你不仅会了解 EVM、字节码和 ABI 的内容和原因，还会了解如何在实际项目中快速生成和使用字节码和 ABI。

如果你愿意，你也可以在 YouTube 上查看这篇博客的视频版本。

[https://www.youtube.com/embed/Z7UNjk_roXI?feature=oembed](https://www.youtube.com/embed/Z7UNjk_roXI?feature=oembed)

## 虚拟机和 EVM

让我们从以太坊虚拟机(EVM)开始。就一会儿，让我们放下“以太坊”，了解一下什么是虚拟机(VM)。通俗地说，虚拟机是一个运行在硬件(一台有内存、存储、处理器和连接电源的操作系统的计算机)上的软件(像所有软件一样)。但与其他软件不同，虚拟机旨在模仿硬件——软件假装成实际的机器，就像虚拟立体声系统的音乐应用程序一样。这就是为什么它是一个“虚拟”机器——它不是物理的，但它模仿物理机器。

我们为什么需要虚拟机？它们是扩展、管理和更新运行软件应用程序的基础架构的有效方式。您可能只需要 20 台物理服务器，每台运行 50 台虚拟机，而不是使用 1000 台物理服务器。您甚至可以让每个虚拟机运行不同的操作系统，这样一个虚拟机可以运行 Windows Server，另一个可以运行 Linux Debian，第三个可以运行 Gentoo Linux，第四个可以运行 ChromeOS！

![Virtual machines vs containers diagram](img/ef893c621227a0fd8a6c51b55a9f9458.png)

<figcaption id="caption-attachment-4274" class="wp-caption-text">Virtual machines that run multiple operating systems on the same underlying hardware.</figcaption>



这样做的好处是，您可以在这些虚拟机上运行多个应用程序，所有这些应用程序都在一台硬件机器上运行，以便更充分地利用机器，更有效地利用机器的处理能力和系统资源，从而降低基础设施成本。

以太坊虚拟机也是虚拟机。但 EVM 的意图是打造一台 [【世界计算机】](https://hir.harvard.edu/vitalik-buterin-ethereum-1/) 【去中心化】，而不是最优地利用硬件资源。EVM 是被称为“节点”的单独的、联网的机器的集合，这些机器试图作为单个机器。每个节点运行一个实现 [以太坊规范](https://ethereum.github.io/yellowpaper/paper.pdf) 的客户端软件，由于它们都相互连接，所以它们形成了一个网络。然后，这个节点网络同步其状态(数据),以便它们一起形成一个始终同步的巨大数据库。数据状态的一致必须通过网络节点来实现，这是通过 [一致](https://en.wikipedia.org/wiki/Consensus_(computer_science)) 算法来完成的。

作为一台分布式虚拟计算机，EVM 运行着名为智能合同的程序。像所有应用程序一样，我们首先编写智能合同，然后编译它，以便它可以被部署(到以太坊区块链网络上)。一旦它存在，代码就不能被改变，因为区块链从设计上来说是不可变的。一旦代码被部署，EVM 就变成了执行代码的环境。EVM 是运行我们部署到其上的智能合约的虚拟机。

我们通常用人类可读的编码语言编写程序(即使当我们开始学习的时候，它们看起来不是很可读！)这是因为人类需要阅读、编辑、维护和调试软件。但是执行代码的机器不会阅读人类可读的语言。机器最擅长处理二进制数据，它看起来像一串 1 和 0。所以在我们写完代码后，我们要求 [编译器](https://en.wikipedia.org/wiki/Compiler) (也是软件)来“编译”它，以便它能在机器上运行。

在 Solidity 中，当我们编译代码时，我们得到两个“工件”:字节码和 ABI。

## Solidity 中的字节码

字节码是我们的可靠性代码被“翻译”成的信息。它包含给计算机的二进制指令。字节码通常是紧凑的数字代码、常量和其他信息。每个指令步骤是被称为“ [”操作码](https://github.com/crytic/evm-opcodes) ”的操作，其通常为一个字节(八位)长。这就是为什么它们被称为“字节码”——单字节操作码。

编写的每一行代码都被分解成操作码，这样计算机在运行我们的代码时就知道该做什么。

在以太坊的世界里，字节码实际上是部署到以太坊区块链的东西。当我们部署到以太网并使用基于浏览器的钱包(如 Metamask)确认交易时，我们实际上可以看到部署的字节码。有很多方法可以将字节码分解成操作码，但那是以后的事了。

![Bytecode smart contracts](img/a91f88d2cff41d940bd6b82e3db3e827.png)

<figcaption id="caption-attachment-4275" class="wp-caption-text">How bytecode looks when we deploy a smart contract using Metamask.</figcaption>



字节码存储在以太坊网络上，当我们与智能合约交互时执行。有很多工具和库(包括官方的 Solidity 编译器 solc)可以帮助你将 Solidity 代码编译成字节码。但是一个快速的方法是在浏览器内 [Remix IDE](http://remix.ethereum.org) 编译智能合约，然后复制 ABI 和字节码。

这里有一个方便的专业技巧来快速生成和复制字节码。您可以点击 [此链接](https://remix.ethereum.org/#url=https://docs.chain.link/samples/PriceFeeds/PriceConsumerV3.sol) 并在您的 Remix IDE 中打开一个支持 Chainlink 价格馈送的 Solidity smart contract。您可以编译然后复制字节码，如下所示。轻松点。

![Remix bytecode](img/122106d8f9522ffc83834a9f2c06d91e.png)

<figcaption id="caption-attachment-4709" class="wp-caption-text">Using Remix to generate bytecode.</figcaption>



## 什么是坚固的 ABI？

你可能听说过 API(应用程序编程接口)。这些基本上是方法、函数、变量和常量的集合，可以用来与库、网络端点、后端服务或其他软件服务和应用程序进行交互。API 是一种以可控、稳定和直观的方式公开软件功能的方式。API 定义了两个软件相互交互的方式——接口。

ABI 是应用程序二进制接口。它们定义了智能合约中可用的方法和变量，我们可以使用这些方法和变量与智能合约进行交互。由于智能合约在部署到区块链之前被转换为字节码，因此我们需要一种方法来了解我们可以启动哪些操作和交互，并且我们需要一种标准化的方法来表达这些接口，以便任何编程语言都可以用于与智能合约进行交互。虽然 JavaScript 是与智能合约交互最常用的语言(主要是因为 JavaScript 是一种前端浏览器语言，我们经常使用前端网页来与智能合约交互)，但您可以使用任何编码语言与智能合约交互，只要您拥有智能合约的 ABI 和一个库来帮助您与任何一个节点通信，从而为您提供进入以太坊网络的入口点。

### 固体 ABI 的结构

因此，ABI 是帮助我们了解方法名称、参数和自变量、我们可以用来与智能合约交互的数据类型，以及智能合约发出的事件的结构的定义。对于函数，下面是在 ABIs 中找到的属性( [来源](https://blog.chain.link/dynamic-nfts-chainlink-keepers-vrf-price-feeds/) ):

 ***   **类型** :指定函数的性质，将是“函数”、“构造函数”、“接收”或“回退”之一。
*   **名称:** 那个函数的名字是什么。
*   **输入** :具有以下模式的对象数组:
    *   **名称** :参数的名称。
    *   **类型** :该参数的类型。
    *   **组件** :当类型为元组时使用。
*   **输出:** 类似输入的对象数组。
*   **状态可变性:** 指定该函数状态可变性的字符串。值为“查看”、“纯”、“查看”、“不可支付”和“可支付”。

自定义错误和事件有非常相似的模式，你可以在这里 研究它们 [。](https://docs.soliditylang.org/en/v0.8.15/abi-spec.html#json)

ABI 表示为 JSON，可以是这样的:

```
[
      {
            "inputs": [],
            "stateMutability": "nonpayable",
            "type": "constructor"
      },
      {
            "inputs": [],
            "name": "getLatestPrice",
            "outputs": [
                  {
                        "internalType": "int256",
                        "name": "",
                        "type": "int256"
                  }
            ],
            "stateMutability": "view",
            "type": "function"
      }
]
```

你可以通过在你的混音 IDE 中再次打开同一个 [支持 Chainlink 价格馈送的 Solidity 智能合约，在 Solidity 中生成你自己的 ABI。然后编译正确的合同并获取 ABI，如下所示。](https://remix.ethereum.org/#url=https://docs.chain.link/samples/PriceFeeds/PriceConsumerV3.sol)

![Remix ABIs](img/dd9b333e127a45c1d4f8016d54b7e15c.png)

<figcaption id="caption-attachment-4710" class="wp-caption-text">Using Remix to quickly generate ABIs.</figcaption>



你会看到 ABI 为你提供了关于某个东西是常规方法还是构造方法的细节，它接受什么输入，它产生什么返回值和数据类型，等等。这是您可以用来解决如何与智能合约交互的模式。当然，你最终会使用像 [EthersJS](https://docs.ethers.io/v5/) 这样的库来与智能合约进行实际的交互，但是这样做你将需要 ABI。

这里有另一种获取 ABI 和字节码的方法:你可以使用 Remix 的“编译细节”标签来获取这些信息和更多信息。只是要小心选择要编译的正确契约，就像编译一个导入的库或智能契约一样，不会生成任何字节码——那只是针对您编写的智能契约！

![ABIs and bytecode in Remix](img/ff1480cdd87c0979af9db9023ae1a496.png)

<figcaption id="caption-attachment-4707" class="wp-caption-text">Getting ABIs and bytecode from Remix.</figcaption>



你现在已经有了一个坚实的心智模型，知道 EVM 是什么，字节码是什么，它做什么，以及为什么 ABI 对智能合同如此重要。重要的是，您还学到了一些漂亮的技术，您可以立即使用这些技术来加快开发过程，并在浏览器中使用智能合同及其编译的工件。

如果您是一名开发人员，并且想要将 Chainlink 集成到您的智能合同应用程序中，请查看 [【区块链教育中心】](https://blockchain.education) ， [开发人员文档](https://docs.chain.link/docs) 或 [联系专家](https://chainlink.typeform.com/to/gEwrPO) 。你也可以 [通过分散的神谕将你的智能合约与现实世界的数据联系起来。](https://docs.chain.link/docs/conceptual-overview/)**