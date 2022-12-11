# Chainlink Hardhat 初学者工具包现在支持类型脚本

> 原文：<https://blog.chain.link/hardhat-starter-kit-typescript/>

我们很高兴地宣布我们最受欢迎的开发人员初学者工具包的新版本:Chainlink Hardhat 初学者工具包现在支持 Typescript！请继续阅读所有新特性的分类。

## 复习工具——链环安全帽初学者工具包

Hardhat 是一个基于 JavaScript 和 TypeScript 的开发环境，使开发人员能够编译、部署、测试和调试 EVM 兼容的智能合约。一年前，我们发布了 Chainlink Hardhat Starter Kit，这是一个单一的 repo，包含开发人员开始使用 Chainlink 构建所需的一切，包括智能合约、测试、高级任务和部署脚本。你可以在 [我们的博客](https://blog.chain.link/using-chainlink-with-hardhat/) 上了解更多。    一年后，这个项目带来了一个全新的 Typescript 版本。要使用初始的 JavaScript 版本，打开您的终端并输入

```
git clone https://github.com/smartcontractkit/hardhat-starter-kit.git
```

要切换到 TypeScript 版本，需要签出到`typescript`分支。

```
git checkout typescript
```

## 里面有什么？

### 打字稿

TypeScript 是一种面向对象的编码语言，是 JavaScript 的超集，旨在克服大型项目的代码复杂性。这意味着所有的 TypeScript 代码都是有效的 JavaScript 代码，而不是相反。TypeScript 的一个主要优点是它是强类型的，这意味着编译器会提醒用户任何与类型相关的错误，从而尽早检测到错误。用 TypeScript 编写的代码比 JavaScript 代码结构更好，也更简洁。

### 测试

这个新版本真正提高了测试水平。测试文件分为两个独立的类别—单元测试和阶段测试。每个测试在等待来自 oracle 网络的回调时监听特定的事件。

每个单元测试都是独立的，来自 oracle 网络的回调响应被来自 [官方链接库](https://github.com/smartcontractkit/chainlink/tree/develop/contracts) 的大量测试合同和助手所模仿。这意味着您可以通过使用以下命令并行运行它们来节省时间:

```
yarn test --parallel
```

另一方面，阶段测试在监听事件的同时，在各种测试网络上使用已经部署的契约实例。没有被嘲笑的合同或模拟的回调。您可以通过将`TS_NODE_TRANSPILE_ONLY` env 变量设置为 1 来防止 ts-node 在每次运行时重新编译和类型检查您的项目，从而提高 Hardhat 的速度。

此外，在测试中，您可以通过取消 Hardhat 配置文件中“forking”属性的注释并将“enabled”设置为 true，在具体块中派生 mainnet。为此，您需要一个存档节点或 JSON RPC 提供者，如 Alchemy。

### 打字机

TypeChain 为您的 Solidity smart contracts 生成 TypeScript typing(d . ts)文件。使用 JavaScript，您不仅需要记住给定智能契约方法或事件的名称，还需要记住它的完整签名。这浪费了时间，并可能引入只有在运行时才会触发的错误。TypeChain 支持更精确和高效的测试、任务和脚本。

### NatSpec

以太坊自然语言规范格式，简称 NatSpec，是一种注释形式，旨在提供丰富的文档来巩固智能合同。这些评论使用了`@notice`、`@params`、`@return`等标签。，可以用来描述从契约本身到函数的几乎任何东西，包括变量、事件甚至修饰符。不仅如此，NatSpec 生成的消息直接显示给最终用户，当他们与您的合同功能交互时，即签署交易时。

### 代码覆盖率

在智能合约开发中，理解代码覆盖率是一项被低估的技能，尤其是当你实践测试驱动开发的时候。它是在运行特定测试套件时，智能合约源代码执行程度的百分比度量。 与测试覆盖率低的项目相比，测试覆盖率高的项目包含未被发现的 bug 的几率更低。 要开发安全的 dApps，你应该达到至少 90%的代码覆盖率。然而，不要试图不惜一切代价追求 100%的覆盖率，因为这会导致意想不到的副作用。

要运行它，请输入:

```
yarn coverage
```

一些 Solidity 文件，像模拟合同，其唯一的目的是作为测试助手，将显示低覆盖率。要从检查中排除它们，请更新“. solcover.js”文件。

### 估算气体

为 EVM 兼容的链编写智能合同意味着用有限的资源编写程序。出于这个原因，开发人员倾向于尽可能地优化他们的代码。Gas 优化可能很棘手，测试套件中添加了一个 Mocha reporter，显示每个单元测试的 gas 使用情况以及方法调用和部署的各种指标。

要估算 gas，只需将一个`REPORT_GAS`环境变量设置为 true，然后运行:

```
yarn hardhat test
```

输出将存储在“gas-report.txt”文件中。如果您想查看美元或其他货币的天然气价格，请从[Coinmarketcap](https://coinmarketcap.com/api/documentation/v1/)中添加一个`COINMARKETCAP_API_KEY`。

### 查看合同大小

您是否曾花费数周时间构建智能合约，却在部署过程中遇到“超出最大代码大小”的错误？这意味着您达到了智能契约代码的大小限制(以太坊测试网为 24，576 字节),并且您需要在部署之前减小契约的大小，或者将它分成几个更小的契约和库。为了避免在开发过程的最后进行大规模重构，添加了一个简单的命令来显示项目中每个智能合约的大小，这样您就可以更早地意识到这种限制并采取相应的行动。

要查看合同大小，运行:

```
yarn run hardhat size-contracts
```

### 模糊测试

由于智能合约的不可更改性，在部署到区块链之前，您总是希望确保测试所有可能的场景。这就是为什么 Chainlink Hardhat 初学者工具包的 TypeScript 版本附带了一个 fuzzer 来查找使用单元测试或阶段测试无法找到的边缘情况。模糊测试是一种生成大量随机场景的技术，以揭示产生意外结果的值。

鼹鼠已经作为模糊测试工具包含在这个初学者工具包中。你需要安装 [Docker](https://www.docker.com/) 并分配至少 8GB 的虚拟内存。要更新此参数，请进入 *【设置】- >资源- >高级- >内存* 。

您也可以从源代码运行鼹鼠。更多信息，请访问 [鼹鼠公共知识库](https://github.com/crytic/echidna) 。

要启动鼹鼠实例，从项目根目录运行下面的命令

```
yarn fuzzing
```

如果是第一次使用，需要等待 Docker 下载[eth-security-toolbox](https://hub.docker.com/r/trailofbits/eth-security-toolbox)镜像。这可能需要一段时间。前面的命令将把项目目录的内容复制到 Docker 容器内的“/src”目录中。    模糊测试仅针对自动化兼容的智能合约示例实现，但您可以随意试验并添加更多测试。该测试的主要目的是实现块时间戳的模糊化。其思想是，无论如何，`performUpkeep`函数不能在设定的时间间隔之前更新`counter`状态变量。

要开始模糊化，运行

```
echidna-test /src/contracts/test/fuzzing/KeepersCounterEchidnaTest.sol --contract KeepersCounterEchidnaTest --config /src/contracts/test/fuzzing/config.yaml
```

几分钟后，您将在控制台中看到测试结果。要退出鼹鼠，请键入

```
exit
```

### VRF v2 的应用

最后增加的当然是新版的 Chainlink 可验证随机函数！Chainlink VRF v2 对您的智能合约的资金和请求随机性的方式进行了多项改进。您将发现如何编写 VRF v2 兼容合同、如何部署它们、订阅管理如何工作以及如何正确测试和使用它们的完整演练。

首先，导航至[【VRF】订阅页面](https://vrf.chain.link/) ，选择网络，连接钱包，点击“创建订阅”。然后将您的“subscriptionId”保存为`VRF_SUBSCRIPTION_ID`环境变量。部署后，返回 VRF 订阅页面，导航到您的订阅，单击“添加消费者”按钮，然后粘贴最近部署的 VRF 消费者合同的地址。一旦完成，您就可以使用`request-random-number`任务:执行一个 VRF 请求

```
yarn hardhat request-random-number --contract insert-contract-address-here --network network
```

一旦你成功地请求了一个随机数，你就可以通过`read-random-number`任务:看到结果

```
yarn hardhat read-random-number --contract insert-contract-address-here --network network
```

如果您遇到错误，请在 VRF 订阅页面上查看请求是否已完成，然后重试。

## 总结

不难理解，TypeScript 是开发大型或复杂应用程序的流行选择，而 JavaScript 通常是小型项目的首选。前往 [Chainlink 公共存储库](https://github.com/smartcontractkit) 开始尝试这个和其他可用的初学者工具包。    访问[chain . link](https://chain.link/)或阅读[docs . chain . link](https://docs.chain.link/)上的文档，了解更多关于 Chainlink 的信息。要讨论整合，[联系专家](https://chainlinkcommunity.typeform.com/to/OYQO67EF)。