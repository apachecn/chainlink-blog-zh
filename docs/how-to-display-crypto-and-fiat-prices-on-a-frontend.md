# 如何使用 JavaScript 或 Solidity 在前端显示加密和固定价格

> 原文：<https://blog.chain.link/how-to-display-crypto-and-fiat-prices-on-a-frontend/>

Web3 开发者通常选择在前端显示平台或协议的原生加密货币，而不是显示到法定货币的转换。不过，尽管关注本币很容易，但展示一种法定兑换可能是一个更好的选择。crypto 中的定价假设了解货币的当前价值，而货币的当前价值往往会波动，这也给新用户带来了不太受欢迎的体验，他们中的许多人更愿意参考美元或本国货币。

幸运的是，使用 [Chainlink 数据馈送](https://chain.link/data-feeds) 在 ETH 和 USD 等货币之间进行转换非常简单，这意味着开发人员只需几个简单的步骤就可以为用户提供更好的体验。本技术教程分解了如何使用 ETH / USD Chainlink 价格源在前端显示加密和菲亚特价格。

## 什么是 Chainlink 数据馈送？

Chainlink 数据馈送是一组智能合约，提供对安全可靠的真实数据源的访问。它们由独立的、声誉良好的、地理上分散的节点运营商提供支持，这有助于确保返回数据的可靠性。例如， [ETH / USD](https://data.chain.link/ethereum/mainnet/crypto-usd/eth-usd) 馈送当前利用 31 个单独的神谕或信息源来确定以美元计的 ETH 价格的当前可信答案。

为什么同样的信息我们需要 31 个来源？使用多个源来聚合经过验证的响应意味着不会有单点故障，并且可以防止数据被篡改。

## 关于供应商的说明

在与智能合约交互时，有必要为 Web3 连接提供一个提供者。通常，这将通过用户的钱包连接来实现。如果这不可用，或者您不需要连接用户的钱包，您可以通过完成相同的功能

```
const provider = new ethers.providers.JsonRpcProvider('RPC_URL_HERE');
```

`RPC_URL_HERE` 可以从 [炼金术](https://www.alchemy.com/) ，[Infura](https://infura.io/)， [道德家](https://moralis.io/) ，或者[quick node](https://www.quicknode.com/)等节点提供者处获得。提供商是我们通往区块链的“门户”。

本教程的另一个要求是以太坊 JavaScript 库。在这个例子中，我使用了[](https://ethers.org/)。您需要安装它，示例才能运行。

```
npm install --save ethers
```

## 使用 JavaScript 部署 ETH / USD 转换

现在，你需要在前端显示 ETH / USD 价格的代码。

如果你愿意跟随，这个例子的代码可以在 [例子回购](https://github.com/smartcontractkit/smart-contract-examples/tree/main/datafeeds-in-svelte) 中找到。按照 README.md 中的说明在本地运行该示例。这个例子使用了[](https://svelte.dev/)，但是同样的概念应该适用于任何前端 JavaScript 框架，比如 React 或者 Vue。您还可以在 smart contract[GitHub repo](https://github.com/smartcontractkit)中使用其他几个初学者工具包。

该代码可通过导入语句在前端使用

```
import { getETHPrice } from '../utils/getETHPrice';
```

然后，将结果存储为 ETH 的美元值

```
value = await getETHPrice();
```

使用该值从 ETH 转换为 USD，假设`ethAmount`是要转换的 ETH 的数量。

```
usdValue = Number(ethAmount * value).toFixed(2);
```

以下是完整的文件:

```
~/utils/getEthPrice.js

import { ethers } from 'ethers';

export async function getETHPrice() {
 const provider = new ethers.providers.JsonRpcProvider('RPC_URL_HERE');

    *// This constant describes the ABI interface of the contract, which will provide the price of ETH*
    *// It looks like a lot, and it is, but this information is generated when we compile the contract*
    *// We need to let ethers know how to interact with this contract.*
    const aggregatorV3InterfaceABI = [
 {
            inputs: [],
            name: 'decimals',
            outputs: [{ internalType: 'uint8', name: '', type: 'uint8' }],
            stateMutability: 'view',
            type: 'function'
 },
 {
            inputs: [],
            name: 'description',
            outputs: [{ internalType: 'string', name: '', type: 'string' }],
            stateMutability: 'view',
            type: 'function'
 },
 {
            inputs: [{ internalType: 'uint80', name: '_roundId', type: 'uint80' }],
            name: 'getRoundData',
            outputs: [
{ internalType: 'uint80', name: 'roundId', type: 'uint80' },
{ internalType: 'int256', name: 'answer', type: 'int256' },
{ internalType: 'uint256', name: 'startedAt', type: 'uint256' },
{ internalType: 'uint256', name: 'updatedAt', type: 'uint256' },
{ internalType: 'uint80', name: 'answeredInRound', type: 'uint80' }
 ],
            stateMutability: 'view',
            type: 'function'
 },
 {
            inputs: [],
            name: 'latestRoundData',
            outputs: [
{ internalType: 'uint80', name: 'roundId', type: 'uint80' },
{ internalType: 'int256', name: 'answer', type: 'int256' },
{ internalType: 'uint256', name: 'startedAt', type: 'uint256' },
{ internalType: 'uint256', name: 'updatedAt', type: 'uint256' },
{ internalType: 'uint80', name: 'answeredInRound', type: 'uint80' }
 ],
            stateMutability: 'view',
            type: 'function'
 },
 {
            inputs: [],
            name: 'version',
            outputs: [{ internalType: 'uint256', name: '', type: 'uint256' }],
            stateMutability: 'view',
            type: 'function'
 }
 ];
    *// The address of the contract which will provide the price of ETH*
    const addr = '0x8A753747A1Fa494EC906cE90E9f37563A8AF630e';
    *// We create an instance of the contract which we can interact with*
    const priceFeed = new ethers.Contract(addr, aggregatorV3InterfaceABI, provider);
    *// We get the data from the last round of the contract* 
    let roundData = await priceFeed.latestRoundData();
    *// Determine how many decimals the price feed has (10**decimals)*
    let decimals = await priceFeed.decimals();
    *// We convert the price to a number and return it*
    return Number((roundData.answer.toString() / Math.pow(10, decimals)).toFixed(2));
}
```

`aggregatorV3InterfaceABI`或者 ABI 组成了我们需要的大部分代码。这是我们将与之交互的契约的数据结构，我们需要让 ethers 知道它。通常，您可以将它们存储在前端项目中单独的 JSON 文件中。在这种情况下，它被包含在 util 文件中，以便将所有内容放在一起。

```
*// The address of the contract which will provide the price of ETH*
    const addr = '0x8A753747A1Fa494EC906cE90E9f37563A8AF630e';
```

该合同地址将根据网络或价格对而变化；您可以使用这些 [数据中的任意一个](https://docs.chain.link/docs/reference-contracts/) 馈合同地址。

```
// We get the data from the last round of the contract 
 let roundData = await priceFeed.latestRoundData();
 // Determine how many decimals the price feed has (10**decimals)
 let decimals = await priceFeed.decimals();
 // We convert the price to a number and return it
    return Number((roundData.answer.toString() / Math.pow(10, decimals)).toFixed(2));
```

与合同的交互非常简单。我们得到了 `latestRoundData` ，从而返回:

*   `roundId` :圆形 ID。
*   `answer` :价格。
*   `startedAt` :本轮开始的时间戳。
*   `updatedAt` :轮次更新时的时间戳。
*   `answeredInRound` :计算答案的回合的回合 ID。我们感兴趣的是 `answer` 。这将是 ETH 的价格，但有一个小小的警告。我们需要知道响应中包含的小数位数。这就是 ``decimals`` 的用武之地。它返回要包含的小数位数。

我们使用`answer`和`decimal`来返回一个 ETH: 的值

```
return Number((roundData.answer.toString() / Math.pow(10, decimals)).toFixed(2));
```

## 输入要使用的值

一旦我们有了一个 ETH 的值，我们就可以用它轻松地将 ETH 转换成美元。在下面的例子中， `rawETH` 是从合同余额中返回的字符串。

```
*<!-- Display the raw ETH -->*
{Number(rawETH).toFixed(8)} ETH
*<!-- Display the ETH converted to USD -->*
$ {(Number(rawETH) * value).toFixed(2)} USD
```

[Chainlink 数据馈送](https://chain.link/data-feeds) 提供可靠的解决方案，使用安全的方法将我们的价值从 ETH 转换为 USD。

## 使用坚固度

到目前为止，在前端应用程序中创建一个实用函数似乎很简单。但是，如果我们可以消除前端开发人员担心定价的需要，并为他们处理定价，会怎么样呢？

只需对您的合同做一些修改，您就可以向最终用户提供当前价格数据，他们只需担心如何连接到您的合同。这简化了所需的前端工作。让我们来看一个示例合同。

```
pragma solidity ^0.8.0;
*// Import the chainlink Aggregator Interface*
import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

*// Create the contract*
contract YourAwesomeContract { 
*// Declare the priceFeed as an Aggregator Interface*
    AggregatorV3Interface internal priceFeed;

    constructor() {
*/** Define the priceFeed*
** Network: Rinkeby*
 ** Aggregator: ETH/USD*
 ** Address: 0x8A753747A1Fa494EC906cE90E9f37563A8AF630e*
 **/*
 priceFeed = AggregatorV3Interface(
            0x8A753747A1Fa494EC906cE90E9f37563A8AF630e
 );
    } 

*// OTHER CONTRACT LOGIC HERE*

    */***
 ** Returns the latest price and # of decimals to use*
 **/*
    function getLatestPrice() public view returns (int256, uint8) {
*// Unused returned values are left out, hence lots of ","s*
 (, int256 price, , , ) = priceFeed.latestRoundData();
 uint8 decimals = priceFeed.decimals();
        return (price, decimals);
 }
}
```

首先，我们导入我们在上面的前端版本中使用的相同的聚合器接口。

```
import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
```

然后，我们需要让聚合器知道我们感兴趣的价格馈送合同的地址。

```
priceFeed = AggregatorV3Interface(
            0x8A753747A1Fa494EC906cE90E9f37563A8AF630e
        );
```

在我们的 `getLatestPrice()` 函数中，我们称之为 `priceFeed.latestRoundData()` 。 `priceFeed` 引用的是我们在上面的 `getETHPrice` util 中使用的同一个契约。它返回:

*   `roundId` :圆形 ID。
*   `answer` :价格。
*   `startedAt` :本轮开始的时间戳。
*   `updatedAt`:轮次更新时的时间戳。
*   `answeredInRound`:计算答案的回合的回合 ID。

这里的代码可能看起来有点奇怪。我们唯一感兴趣的值是`answer`。因此，我们不会将其他值赋给变量。关闭它们可以防止存储未使用的数据。

```
(, int256 price, , , ) = priceFeed.latestRoundData();
```

## 改变前端

现在，价值已经包含在您的智能合约中，我们只需在前端从合约中访问价值。

```
 async function getETHPrice() {
 let [ethPrice, decimals] = await yourAwesomeContract.getLatestPrice();
 ethPrice = Number(ethPrice / Math.pow(10, 
decimals)).toFixed(2);
 return ethPrice;
 }
```

这将为我们合同的消费者创建一个更简单的界面，因为他们不需要了解 oracles 或者导入一个单独的 ABI。

## 总结

在本技术教程中，我们展示了将 Chainlink 数据馈送集成到您的 dApp 中是多么容易，使用户能够轻松引用美元/瑞士法郎价格转换。加密货币和法定货币都有大量的 Chainlink 价格馈送，为开发人员提供了使用他们选择的一对货币来调整这些步骤的机会。

访问[chain . link](https://chain.link/)或阅读[docs . chain . link](https://docs.chain.link/)上的文档，了解更多关于 Chainlink 的信息。要讨论整合，[联系专家](https://chainlinkcommunity.typeform.com/to/OYQO67EF)。