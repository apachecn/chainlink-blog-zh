# Banyan 获得拨款，建立一个分散的 IPFS 钉住服务

> 原文：<https://blog.chain.link/banyan-awarded-chainlink-community-grant/>

[chain link 社区资助计划](https://chain.link/community/grants) 的主要目标之一是通过使用 [混合智能契约](https://blog.chain.link/hybrid-smart-contracts-explained/) 和 [Chainlink 甲骨文](https://blog.chain.link/what-is-chainlink/) 让构建影响世界的应用变得更加容易，从而为全球开发者社区提供支持。为了推进这一目标，该计划从财务上支持开发人员工具的创建，以无缝创建、测试混合智能合同并将其投入生产，并为其持续维护提供资金，确保其保持功能性和安全性。

今天的分散存储解决方案经常迫使用户在可用性和真正的分散/审查阻力之间进行权衡。

为了帮助解决这些问题，我们很高兴地宣布 Banyan——一个帮助用户在 Filecoin 等分散存储网络上经济高效地存储数据的无信任存储市场——获得了 Chainlink 社区资助，以构建一个分散的 IPFS 锁定服务，该服务使用 Merkle proofs 来验证离线文件存储。该服务将通过利用 Chainlink 网络来有效地扩展，以离线计算这些证明。智能合同将能够通过 Chainlink 外部适配器在 Solidity 中进行 API 调用，Chainlink 网络将计算 Merkle 证明，然后通过外部适配器在链上返回该证明，使用户能够轻松且经济高效地验证文件是否存储在 IPFS 上。这项服务将使整个 Web3 生态系统的开发者更容易存储和验证离线数据。

通过这一资助计划，Banyan 旨在完成以下可交付成果:

*   开发一个基于 Rust 的 API，按需提取以太坊数据并在链上发布响应。
*   构建一个开源的 Chainlink 外部适配器，将分散的存储网络无缝连接到以太坊等第一层区块链。
*   扩展外部适配器以使用 IPFS 文件 ID，验证在给定时间段内与文件相关联的链上 Merkle 证明的历史，并返回链上证明成功率。

随着分散式应用不断增加新用户和支持高级功能，它们需要越来越多的数据存储空间。然而，在区块链上存储大量数据的成本很高，而集中式服务器是可以审查的，并且会引入单点故障。该计划旨在使分散式存储网络对 Web3 用户来说更实惠、更可靠，让他们能够获得可验证且经济高效的锁定服务。最终，这是向拥有端到端分散式 Web3 技术堆栈的智能合约开发者迈出的一步，有助于让个人完全控制和拥有自己的数据。

Banyan 是一个链上存储桥、代理和聚合器，帮助用户将来自任何区块链的数据存储在 Filecoin 等分散存储协议上。Banyan 由 Claudia Richoux 和 Maya Krasovsky 创立，他们毕业于芝加哥大学，拥有丰富的网络和金融经验。克劳迪娅获得了学士学位，然后继续攻读硕士学位，最后辍学进入网络 3 行业工作。Claudia 曾在 Trail of Bits 担任密码学审计员和研究员，在 Filecoin 的 Protocol Labs 担任运行时 Go/Rust 开发人员，并为各种 Web3 项目做出了贡献。Maya Krasovsky 主修经济学和社会学，在 Advocate Aurora Health 有投资组合管理经验，在高盛有投资银行经验，在 PrimeDAO 有开放金融经验。

“我们很高兴获得 Chainlink 和 Filecoin 的联合资助。我们相信，让分散存储网络的用户能够验证是否有人真的在 IPFS 上存储文件，是 Web3 生态系统向前迈出的重要一步，”Claudia Richoux 说。“通过使用 Chainlink 网络执行 Merkle 证明计算，作为我们 IPFS 锁定服务的一部分，我们将能够有效地扩展服务，并确保即使在第 1 层块空间需求增长的情况下，用户也能负担得起。”

Maya Krasovsky 说:“我们很高兴收到 Chainlink 社区的资助。“我们相信，构建一个利用 Chainlink 网络高效扩展的分散式 IPFS 锁定服务，将有助于推动分散式存储技术的采用。”

通过 Chainlink 社区资助计划，我们期待继续为 Chainlink 生态系统团队、研究人员和社会影响项目提供支持，这些团队、研究人员和项目都在研究和构建关键工具和基础设施，以加速混合智能合同、安全 [甲骨文网络](https://chain.link/education/blockchain-oracles) 以及改善世界的有效技术的发展。我们将继续支持社区，将其作为 Chainlink 快速增长的关键驱动力，因为只有团结起来，我们才能让普遍连接的混合智能合同成为数字协议的主导形式。

## 关于 Chainlink 赠款计划

如果您想了解更多关于资助计划的信息，请查看我们的 [博客文章](https://blog.chain.link/introducing-the-chainlink-community-grant-program/) ，它进一步阐述了资助计划的目标和提交标准。如果您想参与 Chainlink Grant 计划，请在此 处 [提出申请。](https://chainlinkgrants.typeform.com/to/efEbsq)