# 以太坊名称服务(ENS)和 Chainlink 数据馈送如何简化智能合约开发者体验

> 原文：<https://blog.chain.link/using-ens-and-chainlink-data-feeds/>

[以太坊名称服务](https://ens.domains/) 或 ENS 为区块链地址提供人类可读的名称，为以太坊用户和开发者提供简化的体验。通过清晰的地址，用户可以与智能合同进行交互，而无需使用长字符哈希，长字符哈希可能会令人困惑并导致错误。

Chainlink 正在走向使用 ENS 作为 [数据来源](https://data.chain.link/) 地址的真实来源。考虑到这一点 ，理解 ENS 是什么以及它是如何工作的很重要。

## 什么是 ENS？

ENS 是在以太坊区块链上实现分布式、开放和可扩展命名的服务。或者简单的说，就是一个查找服务。ENS 的工作很简单:它将人类可读的名称映射到机器可读的地址。从这个意义上说，它类似于域名服务，或 DNS，用域名代替 IP 地址。但是 ENS 用人类可读的名称代替了区块链地址，而不是 IP 地址。

ENS 为地址、散列和其他标识符提供命名服务。如果没有 ENS，用户需要知道以太坊区块链上的合同或钱包的完整 64 个字符的地址，以便与之进行交互。

ENS 提供了使用人类可读地址的能力。这些地址可以用作域，从而支持域层次结构，这意味着可以将子域分配给 ENS 地址。

## ENS 对于 Chainlink 是什么意思？

顶级 ENS 域名由被称为注册服务商的智能合同所有。这些注册商提供管理子域名分配的规则。在“. eth”顶级域名上，Chainlink 与 ENS 广泛合作，提供了“data.eth”域名，其中包含 Chainlink 价格馈送地址的可识别索引，便于在以太坊区块链上发现 Chainlink oracle networks。这意味着开发人员能够使用类似“eth-usd.data.eth”的人类可读地址来代替类似“0x 5 F4 EC 3d F9 CBD 43714 Fe 2740 F5 e 3616155 C5 b 8419”的合同地址。

## 在 Javascript 中使用 ENS

在支持 Web3 的 Javascript 库中，使用 ENS 解析域名非常简单。使用[web 3 . js](https://web3js.readthedocs.io/)这和一样简单

```
var address = ens.getAddress('eth-usd.data.eth');
```

其他支持 ENS 的库可以在 [ENS 文档](https://docs.ens.domains/dapp-developer-guide/ens-libraries) 中找到。

## 制作节点哈希

在链上使用 ENS 地址时，事情变得更加有趣。

ENS 文档 [异人引用](https://docs.ens.domains/contract-developer-guide/resolving-names-on-chain) t o 节点散列。节点散列是用递归算法构造的，该算法采用由“.”分隔的域的每个组件，并将它们散列在一起。根据规格为[【EIP-137】](https://eips.ethereum.org/EIPS/eip-137)e 的此算法的伪码为:

```
def namehash(name):
 if name == '':
   return '\0' * 32
 else:
   label, _, remainder = name.partition('.')
   return sha3(namehash(remainder) + sha3(label))
```

名称被拆分成各个组成部分，然后，从最后一个组成部分开始，连接在一起。“eth-usd.data.eth”的结果节点散列将通过以下步骤创建。

```
node = '\0' * 32
node = sha3(node + sha3('eth'))
node = sha3(node + sha3('data'))
node = sha3(node + sha3('eth-usd'))
```

需要注意的是，为了从哈希算法中产生正确的输出，必须首先对名称进行规范化。ENS 要求任何使用它的 mus t 遵循 [UTS46](https://unicode.org/reports/tr46/) 进行规范化和验证。

鉴于此过程的复杂性，建议将节点散列传递给契约，而不是在链上计算它。[eth-ens-name hash](https://www.npmjs.com/package/@ensdomains/eth-ens-namehash)npm 包为您执行规范化和散列。

另外，该图有一个 [API 可用于](https://thegraph.com/hosted-service/subgraph/ensdomains/ens) 查找关于 ENS 域的数据；“labelhash”是包含此信息的特定字段，但还有更多信息可用。

## 解析链上

一旦定义了 ENS 地址的节点散列，就可用于在线解析合同地址。这也很有用，因为对于解析器来说，节点散列和人类可读的地址是相同的。解析器充当我们正在解析的实体的地址的真实来源。在这种情况下，这将是一个链接数据饲料。

在链上可靠性契约中，你需要为 ENS 契约和解析器实现几个接口。

```
abstract contract ENS {
 function resolver(bytes32 node) public virtual view returns (Resolver);
}

abstract contract Resolver {
 function addr(bytes32 node) public virtual view returns (address);
}
```

一旦定义了这些接口，就可以创建一个简单的解析器来将节点散列转换成地址。

```
contract MyContract {
// This is the ENS registry address
// It is the same address for Mainet, Ropsten, Rinkerby, Gorli and other networks;
   ENS ens = ENS(0x00000000000C2E074eC69A0dFb2997BA6C7d2e1e);

 function resolve(bytes32 node) public view returns(address) {
 Resolver resolver = ens.resolver(node);
       return resolver.addr(node);
 }
}
```

## 结束

Chainlink 选择使用 ENS 作为数据源地址的真实来源。ENS 消除了使用长地址的需要，并有助于确保使用正确的区块链地址进行交互。要了解更多关于 ENS 和 Chainlink 的信息，请前往 [Chainlink 文档](https://docs.chain.link/docs/ens/) 。您还可以探索如何使用 ENS 或 Unstoppable Domains 创建您自己的 [NFT 域名](https://blog.chain.link/how-to-create-nft-domain-names/)，您可以使用它们将用户导向您的网站或 dApp。

通过访问 [chain.link](https://chain.link/) 或阅读 [docs.chain.link](https://docs.chain.link/) 上的文档来了解更多关于 Chainlink 的信息。为了讨论整合，[联系专家](https://chainlinkcommunity.typeform.com/to/OYQO67EF)。