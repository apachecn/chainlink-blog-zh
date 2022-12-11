# FilSwan 获得 Chainlink-Filecoin 联合资助，创建多链分散数据存储支付解决方案

> 原文：<https://blog.chain.link/filswan-chainlink-filecoin-joint-grant/>

2021 年，Chainlink 和 Filecoin 启动了 [联合资助计划](https://blog.chain.link/announcing-the-chainlink-and-filecoin-joint-grant-program/) 以加速混合智能合同的开发，该合同将 Chainlink 分散式 oracles 和 Filecoin 分散式存储结合在一个应用程序中。联合资助计划支持团队构建由防篡改文件存储和通用连接支持的混合智能合同应用程序。本质上，这些共同赞助的资助旨在帮助将 [Web3](https://chain.link/education/web3) 堆栈扩展到链上计算之外，以包括分散的链外计算、数据和存储。

考虑到这一点，我们很高兴地宣布 [FilSwan](https://www.filswan.com/) 已获得 Chainlink-Filecoin 联合资助，以创建多链分散数据存储解决方案，允许 Polygon 上的智能合同汇集资金，用于在 Filecoin/IPFS 上存储数据。

使用 FilSwan 的工具，用户可以通过将抵押品作为预付存储费存入 Polygon 智能合同，将文件上传到 IPFS。系统将自动选择一个存储提供商来将该数据备份到 Filecoin 网络，并生成与该数据相关联的唯一的 [内容标识符(CID)](https://spec.filecoin.io/#section-glossary.cid) 。一旦生成了 CID，Polygon 上的多个智能合同就可以指定 CID 和存储提供商，并将资金集中在一起，以生成表明存储交易有效的证明。一旦证明被 Chainlink oracles 转发，表明数据已经存储在 Filecoin 网络上，任何人都可以触发存储提供商的支付，并将抵押品(如果有剩余的话)返还给用户。

随着混合智能合约应用变得越来越先进，产生的数据越来越多，对多链生态系统中大量数据的防篡改存储的需求也在增加，随着区块链之间连接的增加，存储需求也在增加。作为广泛采用的分散存储解决方案，IPFS 和 Filecoin 已被用于存储来自多个领域的各种数据，包括 NFT 元数据、科学数据、体育数据、市场分析等。然而，为其他区块链的多个智能合同支付存储费用的问题仍未解决。

fils wan 的多链数据存储支付解决方案由 Chainlink 的无缝连接提供支持，将使用户、开发者和 Dao 能够汇集资金，通过分布式网络存储和共享关键数据。 代码将被上传到 [FilSwan 的 GitHub](https://github.com/filswan/) 并定期更新计划中的功能增强，如改进的密钥管理和 S3 兼容的文件导入服务。

FilSwan 通过将数据、计算和支付集成到一个套件中，为 Web3 项目提供多链解决方案。自 FilSwan 项目开始以来，该团队已经获得了一系列广泛的资助和奖励，包括加拿大政府的高科技研发资助，加拿大自然科学基金会(NSERC)区块链到人工智能云计算研究资助，加拿大信息技术和集成系统数学组织(Mitacs)人工智能医学识别项目资助，HackFS Chainlink 奖，等等。FilSwan 团队还与惠普、戴尔、华硕、亚马逊和英伟达建立了企业和供应合作伙伴关系。

fil swan 在分散存储和边缘计算领域拥有丰富的经验，是 Filecoin/IPFS 生态系统中的活跃社区成员，帮助社区开发者和企业加入 Filecoin 网络和分散存储行业。FilSwan 之前的产品包括人工智能云计算平台 [NBAI](https://nbai.io/) 、Filecoin 挖掘节点 [NBFS 池](https://nbfspool.com/#/) ，以及 Filecoin 网络的存储 marketplace。

> “我们很高兴获得 Chainlink-Filecoin 联合资助，用于使用 Chainlink 和 Filecoin 创建分散存储的多链支付解决方案。作为一个在分散存储技术和 Filecoin 基础设施开发方面拥有丰富经验的团队，我们期待使用 Chainlink oracles 创建工具，在多链智能合约生态系统中推进分散存储的连接性。”–查尔斯·曹，FilSwan 的创始人

通过 Chainlink 社区资助计划，我们期待继续为更多 Chainlink 生态系统团队和研究人员提供支持，他们正在研究和构建关键工具和基础设施，以加速开发 [混合智能合约](https://blog.chain.link/hybrid-smart-contracts-explained/) 和安全[Oracle](https://chain.link/education/blockchain-oracles)网络。我们将继续支持社区，将其作为 Chainlink 快速增长的关键驱动力，因为只有团结起来，我们才能使混合智能合同成为数字协议的主导形式。

## 关于 Chainlink 赠款计划

如果你想了解更多关于资助项目的信息，请阅读 [这篇博文](https://blog.chain.link/introducing-the-chainlink-community-grant-program/) ，它进一步阐述了资助项目的目标和提交标准。如果您想参与 Chainlink Grant 计划，请在此 处 [提出申请。](https://chainlinkgrants.typeform.com/to/efEbsq?typeform-source=blog.chain.link)