# 祝贺 ETHGlobal 的 2020 年黑客马拉松获胜者

> 原文：<https://blog.chain.link/ethonline-2020-chainlink-hackathon-winners/>

*chain link 2021 秋季黑客马拉松于 10 月 22 日拉开帷幕。* [*今天报名。*](https://chain.link/hackathon?utm_medium=referral&utm_source=chainlink-blog&utm_campaign=fall-2021-hackathon&utm_content=congratulations-to-ethglobals-2020-hackathon-winners)

作为 eth global 2020 年 [ETHOnline Hackathon](https://ethglobal.online/) 的赞助商，Chainlink 向所有参与者和 Hackathon 获奖者致以衷心的感谢和祝贺。ETHGlobal 的虚拟活动是一个很好的机会，可以看到来自更广泛的以太坊社区的各种各样的开发。作为赞助商，我们很高兴能够支持开发人员利用 [Chainlink 的](https://chain.link/)分散式甲骨文网络提高智能合约的可用性和连接性。

今天，我们很高兴地宣布五个杰出的获奖者提交的集成 Chainlink 技术。五个获奖项目中的每一个都将获得价值 1000 美元的奖金。

再次感谢所有使 ETHOnline 黑客马拉松成功的参与者、赞助商和评委，特别感谢 [ETHGlobal](https://ethglobal.co/) 继续支持开发安全的去中心化技术的开发人员和团队。

## chainlink etvonline 黑客马拉松获胜者

下面的获奖者按字母顺序突出显示。

### Iroiro:去中心化的创作者支持平台

由 Tsukasa Noguchi 和 Toshiaki Takase 提交的基于以太坊的创作者支持平台 Iroiro ，允许创作者使用 [Audius](https://audius.co/) 分散音乐平台将奖励代币分发到他们粉丝的钱包地址。因为向追随者发送代币的汽油费可能会令人望而却步，所以 Iroiro 使用 IPFS 将奥迪斯支持者的信息保存为 JSON 文件。支持者可以在方便的时候提取象征性的奖励。

Iroiro 团队创建了一个 Chainlink 外部适配器来连接 Iroiro 和 IPFS。当支持者提出取款请求时，智能合约使用 Chainlink 检查 JSON 文件中支持者的地址，以批准取款地址。

Iroiro 模型允许支持者确定代币属性，如代币名称、符号和总供应量，以及独特的属性，如代币分配、归属和赌注能力。这种选项的灵活性使创作者能够更好地控制新颖的、秘密经济的粉丝参与模式的令牌分发。

[https://www.youtube.com/embed/HyvpCEV-mc8?feature=oembed](https://www.youtube.com/embed/HyvpCEV-mc8?feature=oembed)



<figcaption>Iroiro – Chainlink Sponsored ETHOnline Hackathon winner.</figcaption>



### 叠加:长/短 DeFi 数据流导数

开发人员 Michael Feldman 使用一种创新的方法来预测市场，创建了 [Overlay](https://hack.ethglobal.co/showcase/overlay-reclnZZ0Y0UnCzlQ6) ，这是一种协议，允许用户使用直接令牌调整作为支付机制来做多或做空某个事件的可能性。用户不是对其他市场参与者下注，而是通过对数据流下注合成叠加令牌来设置头寸，预测数据流的趋势方向(例如，价格数据向上或向下移动)。当用户退出头寸时，[智能合约](https://chain.link/education/smart-contracts)根据数据流结果，将覆盖令牌烧录或存入账户。

Overlay 使用 Chainlink oracles 根据链外数据确定事件的可能性。现场演示使用[【BTC/美元](https://data.chain.link/btc-usd)、[ETH/美元](https://data.chain.link/eth-usd)、[戴/美元](https://data.chain.link/dai-usd)和 [FastGas](https://data.chain.link/fast-gas-gwei) Chainlink 价格馈送作为参考流。Overlay 还使用 ERC-1155 令牌标准，该标准允许用户在二级市场上出售他们的头寸，同时保留唯一的头寸标识符，如锁定价格、杠杆系数以及头寸是做多还是做空。

[https://www.youtube.com/embed/IkqAWg42JLk?feature=oembed](https://www.youtube.com/embed/IkqAWg42JLk?feature=oembed)



<figcaption>Overlay – Chainlink Sponsored ETHOnline Hackathon winner.</figcaption>



### 卢比:印度卢比(印度卢比)稳定的衍生货币

由 Vijay Lakshminarayanan 开发的印度卢比(INR) stablecoin 项目 [Rupia](https://hack.ethglobal.co/showcase/rupia-derivative-inr--recW4prsiYa8qzffK) 是一个概念验证，允许用户将印度卢比法定货币转换为 DAI，然后用 DAI 来铸造卢比 stablecoins。Rupia 使用 Chainlink DAI/INR 价格馈送来计算从 DAI 铸造的 Rupia 代币的数量。

该项目有三个主要组成部分:

*   使用 [Transak](https://transak.com/) 将 INR 转换为 Matic 网络 DAI 的菲亚特至 DAI 网关。Rupia 使用 React 前端将 Transak 用户界面转换为 INR 特性。
*   链环 DAI/INR 价格馈送。Chainlink oracle 网络从[数据源(示例中为 crypto compare)](https://www.cryptocompare.com/coins/dai/markets/INR)提取价格数据，然后将价格交付给鲁皮亚 ERC-20 智能合约。
*   Rupia ERC-20 智能合约使用 [OpenZeppelin](https://openzeppelin.com/) 生成令牌，令牌是基于 DAI/INR 价格馈送铸造的。

Rupia 为 dApps 包括其他基于法定货币的稳定货币奠定了技术基础，这些货币在加密货币市场中的风险较小。

[https://www.youtube.com/embed/bS6OJptNgug?feature=oembed](https://www.youtube.com/embed/bS6OJptNgug?feature=oembed)



<figcaption>Rupia – Chainlink Sponsored ETHOnline Hackathon winner.</figcaption>



### SecretPay:从 PayPal 和 Revolut 私人购买 ETH

Francesco Cremonato 创建的[secret pay](https://hack.ethglobal.co/showcase/secretpay-reca2E8KsK6BZMTZV)使用私人托管合同，允许用户使用他们的 [PayPal](https://www.paypal.com/) 或 [Revolut](https://www.revolut.com/) 账户进行点对点 ETH 购买。具体来说，SecretPay 使用 Chainlink oracles 将 PayPal 或 Revolut 发票发送到纺织品邮箱，使开发人员能够使用钱包密钥对作为地址，将加密的点对点邮箱集成到 Web3 应用程序中。

使用 SecretPay，买家用 Paypal 或 Revolut 建立购买发票，而卖家将 ETH 存入托管账户。SecretPay 将买方的发票发送给 Chainlink oracles，后者读取发票并将发票数据传输到买方和卖方的纺织品邮箱。买方和卖方确认发票，然后买方在 PayPal 或 Revolut 上执行购买，SecretPay 将卖方托管的 ETH 发送给买方。这种解决方案是一种将广泛采用的金融科技服务与快速增长的 DeFi 空间连接起来的新方法。

[https://www.youtube.com/embed/FalHypwBLlo?feature=oembed](https://www.youtube.com/embed/FalHypwBLlo?feature=oembed)



<figcaption>SecretPay – Chainlink Sponsored ETHOnline Hackathon winner.</figcaption>



### Unipeer: 使用开放银行 API 的菲亚特至以太坊入口

Unipeer 团队 Shaleen Jain 和 Avie Kakkar 已经创建了一种[点对点的启动和关闭方法](https://hack.ethglobal.co/showcase/unipeer-rec2wu97nPZ2cI0lp)，通过加密的开放银行 API 从现有的菲亚特银行账户中提取资金。Unipeer 调用 API 来访问支付信息，而不会将敏感数据泄露到公共区块链上。在过去几年中，使用开放银行 API 的金融科技应用程序有了显著增长，这是许多创新金融科技产品的背后，这些产品支持使用传统银行 rails 的新应用程序，如 Plaid、Yodlee 等。

Unipeer 使用定制的 [Chainlink 外部适配器](https://blog.chain.link/build-and-use-external-adapters/)将开放银行 API 从沙盒 API 环境连接到智能合约。外部适配器连接到 Comptroller smart contract，后者通过银行 API 启动 fiat 支付流程。外部适配器还连接到托管合同，以确认法定支付并释放卖方的托管资金。

[https://www.youtube.com/embed/311BvncwzdE?feature=oembed](https://www.youtube.com/embed/311BvncwzdE?feature=oembed)



<figcaption>Unipeer – Chainlink Sponsored ETHOnline Hackathon winner.</figcaption>



* * *

恭喜获奖者，感谢所有在 eth global 2020 黑客马拉松中使用 Chainlink 的开发者！我们很高兴看到您参与未来的黑客马拉松，因为我们将继续支持创新和安全的智能合同开发。

如果你是一名准备参加下一届黑客马拉松的开发者，你需要资源将你的应用连接到 [Chainlink Price Feeds](https://docs.chain.link/docs/using-chainlink-reference-contracts) 、 [Chainlink VRF](https://docs.chain.link/docs/chainlink-vrf) 或[访问任何 API](https://docs.chain.link/docs/request-and-receive-data) ，访问[开发者文档](https://docs.chain.link/)并加入 [Discord](https://discordapp.com/invite/aSK4zew) 中的技术讨论。如果您想安排一次电话会议来更深入地讨论集成，请联系此处的。

[网站](https://chain.link/) | [简讯](https://chn.lk/newsletter) | [推特](https://twitter.com/chainlink)|[Reddit](https://www.reddit.com/r/Chainlink/)|[YouTube](https://www.youtube.com/channel/UCnjkrlqaWEBSnKZQ71gdyFA)|[电报](https://t.me/chainlinkofficial) | [事件](https://blog.chain.link/tag/events/) | [GitHub](https://github.com/smartcontractkit/chainlink)

*[chain link 2021 秋季黑客马拉松](https://chain.link/hackathon) 于 2021 年 10 月 22 日开赛。无论您是开发人员、创作人员、艺术家、区块链专家，还是该领域的新手，这个黑客马拉松都是启动您的智能合同开发之旅并向行业领先的导师学习的最佳场所。立即锁定您的席位，争夺超过$ 300，000 的奖金。T9】*

[Sign up today](https://chain.link/hackathon?utm_medium=referral&utm_source=chainlink-blog&utm_campaign=fall-2021-hackathon&utm_content=congratulations-to-ethglobals-2020-hackathon-winners)