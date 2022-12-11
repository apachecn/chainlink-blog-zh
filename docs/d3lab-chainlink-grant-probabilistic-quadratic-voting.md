# D3LAB 获得 Chainlink 资助，以增强 DAOs 的抗 Sybil 二次投票系统

> 原文：<https://blog.chain.link/d3lab-chainlink-grant-probabilistic-quadratic-voting/>

由区块链和智能合同推动的一种新的社会协调模式，分散自治组织(Dao)已经成为促进社区组织的一种重要手段。据[deep DAO](https://deepdao.io/organizations)报道，DAO 担保的国库资产的总价值目前超过 80 亿美元，超过 40 万个地址已经提出或投票支持 DAO 治理提案。像 [快照](https://snapshot.org/) 和 [理货](https://www.withtally.com/) 这样的投票服务支持许多 Dao 建立和组织他们的社区。虽然 Dao 仍处于萌芽阶段，但作为围绕业务、共同事业或共同利益构建社区的引擎，它们显示出了巨大的潜力。

[D3LAB](http://d3lab.xyz) 凭借一个名为概率正交投票(PQV)的新的抗 Sybil 二次投票系统的提案获得了[chain link Fall Hackathon 2021](https://blog.chain.link/chainlink-fall-2021-hackathon-winners/)DAO 奖，该提案解决了 DAO 的二次投票系统中的 Sybil 攻击问题。由 Sanghyeon Park，Elin Park，莫峻·李和 In-gun Kim 构建的 PQV 通过利用[chain link VRF](https://chain.link/chainlink-vrf)为现有的二次投票系统添加了概率元素，从而产生了运行 Sybil 攻击对攻击者无利可图的模拟场景。D3LAB 在[Governor-C smart contract](https://github.com/D3LAB-DAO/Governor-C)中实现了该系统，这是一个完全去中心化的、基于 PQV 和 Chainlink VRF 的抗 Sybil 二次投票系统。

继他们的黑客马拉松成功之后，D3LAB 团队希望继续追求他们增强 DAO 治理流程的愿景。考虑到这一点，我们很高兴地宣布 D3LAB 获得了 Chainlink 社区资助，以在开源的基础上开发 PQV 和 Governor-C smart 合约的升级版本，这样其他开发人员和 Dao 也可以受益。

作为这笔资助的一部分，D3LAB 团队将开发 PQV 和 Governor-C 的升级和更具成本效益的版本，并将该系统应用于其他类型的 Web3 治理技术，如 [二次资助](https://wtfisqf.com/) 和 [二次注意力支付](https://vitalik.ca/general/2019/12/07/quadratic.html) 。D3LAB 还将研究使用 PQV 升级利益证明和授权利益证明网络的可能性。早期的假设是，PQV 将有助于提高利益证明网络的有效性和公平性，该团队将进行早期研究来测试这一理论。

Dao 长期以来一直受到“一元一票”和“一人一票”投票制度带来的问题的限制，因为这些制度经常使治理流程面临 [Sybil 攻击](https://en.wikipedia.org/wiki/Sybil_attack) 。D3LAB 的 PQV 系统是一个改进的二次投票系统，它是完全分散的和抗 Sybil 的，并使用由 Chainlink VRF 提供的可验证的随机性来为二次投票添加概率元素。

Dao 目前面临的另一个问题是缺乏现成的基础设施。凭借其固有的[](https://blog.chain.link/defis-permissionless-composability-is-supercharging-innovation/)，开源智能合约允许在 [去中心化金融(DeFi)](https://chain.link/education/defi)[NFTs](https://chain.link/education/nfts)和其他基于区块链的生态系统中进行快速和无摩擦的创新循环。Dao 可以从 PQV 这样的开放标准中受益匪浅，开发者和社区可以无缝集成这些标准来增强他们的治理系统，并发明新的方法来组织在线社区，而不必从头开始构建自己的基础架构。

D3LAB 团队成员 In-gun Kim 在评论拨款时表示:“我们很高兴收到 Chainlink 社区的拨款，用于增强我们的概率二次投票系统并开发 Governor-C 合同的升级版本。希望改善其治理机制的 Dao 将能够无缝集成我们的投票系统，并利用 Sybil-resistant quadratic voting 来更加公平地做出集体决策。我们预计这套工具将有助于创建一个更健康的 DAO 生态系统，并最终通过增强其他治理技术(如二次融资)来促进 Web3 领域的更多创新。”

通过 Chainlink 社区资助计划，我们期待继续为更多 Chainlink 生态系统团队和研究人员提供支持，他们正在研究和构建关键工具和基础设施，以加速开发 [混合智能合约](https://blog.chain.link/hybrid-smart-contracts-explained/) 和安全[Oracle](https://chain.link/education/blockchain-oracles)网络。我们将继续支持社区，将其作为 Chainlink 快速增长的关键驱动力，因为只有团结起来，我们才能使混合智能合同成为数字协议的主导形式。

## 关于 Chainlink 赠款计划

如果你想了解更多关于资助项目的信息，请阅读 [这篇博文](https://blog.chain.link/introducing-the-chainlink-community-grant-program/) ，它进一步阐述了资助项目的目标和提交标准。如果您想参与 Chainlink Grant 计划，请在此 处 [提出申请。Chainlink 社区赠款以现金和/或 link 的形式提供。](https://chainlinkgrants.typeform.com/to/efEbsq?typeform-source=blog.chain.link)