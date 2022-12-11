# 获取可靠智能合约的指数价格

> 原文：<https://blog.chain.link/get-index-prices-in-solidity-smart-contracts/>

资产价格会经历巨大的波动，尤其是在流动性差的市场。这是围绕反映广泛市场覆盖面并高度抵制低流动性价格源操纵的汇总价格数据建立智能合约非常重要的主要原因。对于将[甲骨文](https://blog.chain.link/what-is-the-blockchain-oracle-problem/)集成到 dApps 中的开发者，以及希望分散投资以获得更广泛市场敞口的消费者来说，波动性是一个重要的考虑因素。对一些人来说，这就是指数基金的用处。

指数基金或指数是跟踪一组资产而不是单一资产的金融产品。通过将多种资产组合在一起，指数基金为消费者提供了一种简单而廉价的多元化投资方式。有了指数，消费者只需支付一笔费用，就可以立即获得一篮子资产的所有权。与单独购买多种资产或将资金委托给一位经理相比，指数既简单又便宜。

就像在传统金融领域一样， [DeFi](https://blog.chain.link/analyzing-the-defi-ecosystem-and-the-many-ways-chainlink-can-accelerate-adoption/) 用户希望通过指数轻松实现多样化。与所有 DeFi 产品一样，准确可靠的定价数据至关重要。如果指数价格受到操纵，并产生人为波动，指数的目的就落空了，它不再为投资者提供持续的避风港。一个指数可以由许多具有低波动性的高流动性资产组成，但如果该指数的价格供给缺乏流动性或容易受到操纵，攻击者只需操纵该价格供给，而不是指数中资产的价格。因此，提供指数价格的 oracle 机制成为 DeFi 指数失败的潜在单一来源。

令人欣慰的是，Chainlink 广泛采用的[数据馈送](https://data.chain.link/)解决了这个问题，它为传统指数提供了广泛的市场、预先汇总的分散价格预测，如富时 100 指数(金融时报股票交易所指数)、日经 225 指数(东京股票交易所指数)、sCEX 指数(合成交易所指数)和 sDeFi 指数等 DeFi 指数。

在本指南中，我们将介绍如何在您的智能合约中快速检索 Chainlink 的防篡改索引价格馈送，以便您可以围绕新的链上数据集构建更安全的 DeFi 应用程序。

## 初始化订阅源

任何 Chainlink 数据提要的第一步都是导入聚合器接口，然后用构造函数中的正确地址初始化提要。在本例中，我们使用以太坊上的 FTSE 100 feed 地址进行初始化，但也可以使用任何其他 feed 地址。

**注意:**本教程使用的是 [Remix](https://remix.ethereum.org/?#gist=eae89b2fd30a2d3da06d1e2745122006) 语法。对于 nodejs/python，请参考 [Chainlink 文档](https://docs.chain.link/docs/get-the-latest-price)。

```
pragma solidity ^0.6.7;

import "https://github.com/smartcontractkit/chainlink/blob/master/evm-contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract indexFeed{

    AggregatorV3Interface internal ftseFeed;

    /**
     * Aggregator: FTSE
     * Address: 0xE23FA0e8dd05D6f66a6e8c98cab2d9AE82A7550c
     */
    constructor() public {
        ftseFeed = AggregatorV3Interface(0xE23FA0e8dd05D6f66a6e8c98cab2d9AE82A7550c);
    }
```

## 检索值

一旦提要被初始化，我们只需要一个函数来检索价格数据。

```
//Returns the latest Supply info
    function getLatestPrice() public view returns (int) {
        (
            uint80 roundID, 
            int price,
            uint startedAt,
            uint updatedAt,
            uint80 answeredInRound
        ) = ftseFeed.latestRoundData();
        return price;
    }
}
```

由于数据已经由 Chainlink oracle 节点的网络在链上聚合和提交，我们只需要一个简单的查看函数来访问数据，因此没有汽油成本。还有一些其他字段，如 ID 和检索数据的聚合回合的时间，但是在这种情况下，我们只关心价格。

## 结论

指数基金有助于投资者轻松、廉价地分散投资，通过将资产组合在一起降低波动性。然而，提供这些指数的 DeFi 产品如果使用集中的或低质量的价格数据来源作为整体指数，则容易受到操纵。Chainlink 通过让智能合约开发人员能够极其简单地检索指数价格数据，来确保数据质量和数据交付的安全性，这些指数价格数据是由数百个来源汇总的，并由经过安全审查的 oracle 节点组成的分散网络进行验证。DeFi 用户需要各种强大的金融产品，Chainlink 可以帮助开发人员提供最安全可靠的版本。

如果你是一名开发人员，想要将 Chainlink 集成到你的智能合同应用程序中，请查看 [Chainlink 开发人员文档](https://docs.chain.link/docs)或[点击这里](https://chainlink.typeform.com/to/gEwrPO)。

### 相关主题

*   [取实外汇汇率](https://blog.chain.link/fetch-foreign-exchange-rates-in-solidity-smart-contracts/)
*   [将任何 API 连接到您的智能合约](https://blog.chain.link/apis-smart-contracts-and-how-to-connect-them/)
*   [chain link 价格馈送中的 3 级数据聚合](https://blog.chain.link/levels-of-data-aggregation-in-chainlink-price-feeds/)