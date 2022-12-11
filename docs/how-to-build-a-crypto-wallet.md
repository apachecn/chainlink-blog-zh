# 如何建立一个加密钱包

> 原文：<https://blog.chain.link/how-to-build-a-crypto-wallet/>

当你开始你的 Web3 之旅时，你要做的第一件事就是安装一个加密钱包。钱包是一个必不可少的软件，使您能够存储加密货币并与之互动。在这个技术教程中，我们将探索不同类型的加密钱包，并学习如何建立自己的。

管理资金有风险，本文的目的只是解释加密钱包软件背后的技术概念。强烈建议您使用已经建立的带有开源和审计代码库的 wallet，如 MetaMask、Ledger 或 Argent。

## 入门

为了这个演示的目的，我们将创建一个简单的 Node.js 应用程序并使用 [以太加密](https://github.com/ethereum/js-ethereum-cryptography) 和[ethers . js](https://docs.ethers.io/v5/getting-started/)库。首先创建一个名为`my-crypto-wallet`的新的空文件夹，并导航到它。然后通过键入:创建一个新项目并安装必要的依赖项

```
yard init -y
yarn add ethereum-cryptography ethers
```

要跟进，请查看位于`My Crypto Wallet`文件夹下的 [Chainlink 智能合同范例库](https://github.com/smartcontractkit/smart-contract-examples)中的完整工作范例。 

```
git clone https://github.com/smartcontractkit/smart-contract-examples.git
cd smart-contract-examples/my-crypto-wallet
```

## 都是密码学

区块链正在解决信任的问题。有了加密货币，你可以独自控制你的资金，而不是银行或其他第三方。加密钱包只是一个管理你的资金的工具，你可以很容易地从一个钱包服务切换到另一个。这一切都可能是密码学的结果:我们不需要相信任何人，我们可以轻松地用密码验证真相，保护自己的资金。

在本次演示中，我们将重点关注以太坊虚拟机(EVM)帐户，因为熟悉 EVM 的开发人员很多。帐户只是一个私钥和公钥的加密对。假设你的账户是你的信用卡。公钥就是你的信用卡号，而私钥就是你的个人识别码。您可以轻松地与任何人共享您的公钥，但私钥必须只对您自己保密。如果您的私钥被泄露，您将面临永久失去所有资金的风险。和任何借记卡一样，只要你的账户上有资金，你就可以花钱。

生成新钱包的第一步是写下“种子短语”或助记符。这将生成帐户的其余部分(成对的私钥/公钥),并且是恢复您的加密钱包的唯一方法。如果您删除了移动钱包或丢失了手机，硬件钱包损坏，或者您使用了 web 浏览器扩展钱包而计算机死机，您的种子短语将允许您在手机、浏览器或硬件设备上设置新的钱包，并重新获得对您的加密的完全访问权。

在项目中创建一个新的 JavaScript 文件，并将其命名为`01_newAccount.js`。让我们创建一个函数来生成助记符。比特币改进提案 39，简称 BIP-39，是一个生成助记符或种子短语的规范。它以一个随机数开始，称为熵，需要是 32 位的倍数(强度% 32 == 0)，长度在 128-256 位之间。128 位长的熵将产生由 12 个单词组成的助记符，而 256 位长的熵将产生 24 个单词的助记符。熵越大，产生的助记词就越多，你的钱包就越安全。

```
const { generateMnemonic, mnemonicToEntropy } 
= require("ethereum-cryptography/bip39"); const { wordlist } = require("ethereum-cryptography/bip39/wordlists/english"); 
function _generateMnemonic() {
  const strength = 256; // 256 bits, 24 words; default is 128 bits, 12 words
  const mnemonic = generateMnemonic(wordlist, strength);
  const entropy = mnemonicToEntropy(mnemonic, wordlist);
  return { mnemonic, entropy };
}
```

如果您有一个帐户的私钥，您可以将其移动到另一个钱包应用程序并在那里使用，同时您可以使用您的种子短语来恢复所有钱包帐户，只需通过助记符逐个生成它们即可。

BIP-32 是用于创建分级确定性(HD)钱包的规范，其中单个密钥可用于生成整个密钥对树。这个单独的键作为树的根，它总是由完全相同的单词组合生成，也称为助记符或种子短语。根密钥实际上为帐户生成所有其他私钥，它们都可以通过这个根密钥恢复。由于人类更容易记住 12 或 24 个单词，而不是复杂的随机数和字符流，我们倾向于说种子短语恢复钱包，而实际上它是用于生成根密钥，然后可以派生出私钥树。

```
const { HDKey } = require("ethereum-cryptography/hdkey");
 function _getHdRootKey(_mnemonic) {
  return HDKey.fromMasterSeed(_mnemonic);
}
```

账户的私钥是一个 256 位长的 0 和 1 的流。如果你掷硬币 256 次，写 1/0 表示正面/反面，很有可能你会产生一个目前没有人使用的私钥。但是，当您可以使用种子短语创建钱包帐户时，不建议生成随机私钥。

```
function _generatePrivateKey(_hdRootKey, _accountIndex) {
  return _hdRootKey.deriveChild(_accountIndex).privateKey;
}
```

以太坊和比特币使用 secp256k1 椭圆曲线进行加密计算。每个帐户的公钥都是使用椭圆曲线数字签名算法(简称 ECDSA)从相应的私钥中导出的。通过将 ECDSA 应用于私钥，我们得到一个 64 字节的整数，这是两个 32 字节的整数，它们表示 secp256k1 椭圆曲线上一点的 X 和 Y，并连接在一起。这种算法背后的数学允许软件容易地计算给定私钥的公钥，而相反的过程是不可能的。人们不能在 secp256k1 椭圆曲线上使用 ECDSA 计算给定公钥的私钥。

```
const { getPublicKey } = require("ethereum-cryptography/secp256k1"); 
function _getPublicKey(_privateKey) {
  return getPublicKey(_privateKey);
}
```

一旦我们有了公钥，我们就可以计算账户地址。以太坊在所有网络中使用相同的地址，包括汇总网络、测试网络和 mainnet。用户指定他们想在钱包软件中使用的网络。

为了根据公钥计算地址，我们需要对公钥应用 Keccak-256 哈希算法，并取结果的最后(最低有效)20 个字节。

```
const { keccak256 } = require("ethereum-cryptography/keccak");
 function _getEthAddress(_publicKey) {
  return keccak256(_publicKey).slice(-20);
}
```

为了生成新的钱包助记符和第一个账户，我们需要调用所有之前定义的函数。

```
const { bytesToHex } = require("ethereum-cryptography/utils");

async function main() {
  const { mnemonic, entropy } = _generateMnemonic();
console.log(`WARNING! Never disclose your Seed Phrase:\n ${mnemonic}`); 
  const hdRootKey = _getHdRootKey(entropy);
  const accountOneIndex = 0;
  const accountOnePrivateKey = _generatePrivateKey(hdRootKey, accountOneIndex);
  const accountOnePublicKey = _getPublicKey(accountOnePrivateKey);
  const accountOneAddress = _getEthAddress(accountOnePublicKey);
  console.log(`Account One Wallet Address: 0x${bytesToHex(accountOneAddress)}`);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

要运行该程序，请在您的终端中输入以下命令:

```
node 01_newAccount.js 
```

前面的程序创建了一个新的钱包。如果你想用你的记忆恢复它，创建一个新文件，命名为`02_restoreWallet.js`，并粘贴以下代码:

```
const { mnemonicToEntropy } = require("ethereum-cryptography/bip39");
const { wordlist } = require("ethereum-cryptography/bip39/wordlists/english");
const { HDKey } = require("ethereum-cryptography/hdkey");
const { getPublicKey } = require("ethereum-cryptography/secp256k1");
const { keccak256 } = require("ethereum-cryptography/keccak");
const { bytesToHex } = require("ethereum-cryptography/utils");

async function main(_mnemonic) {
  const entropy = mnemonicToEntropy(_mnemonic, wordlist);
  const hdRootKey = HDKey.fromMasterSeed(entropy);
  const privateKey = hdRootKey.deriveChild(0).privateKey;
  const publicKey = getPublicKey(privateKey);
const address = keccak256(publicKey).slice(-20); 
  console.log(`Account One Wallet Address: 0x${bytesToHex(address)}`);
}

main(process.argv[2])
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

要运行它，请键入`node 02_restoreWallet.js`和您的种子短语。

```
node 02_restoreWallet.js "here is where you should put your twelve-word mnemonic" 
```

## 冷热钱包的区别

当您想要发送您的一些数字资产(硬币、代币、NFT 等)时。)，您使用 ECDSA 和您的私钥对数据进行数字签名，并在发送给接收方之前对其进行加密。接收者可以使用您的公钥来验证签名。

像 MetaMask 和比特币基地钱包这样的热门钱包或软件钱包通常会将账户私钥存储在它们的服务器和浏览器的本地存储器中。Ledger 或 Trezor 等冷钱包或硬件钱包是保存您的私钥并使其离线的物理设备。要签署交易，您必须连接您的硬件钱包，使其在线，并实际点击它进行确认。一旦你完成交易，你断开你的钱包和你的钥匙回到离线状态。重要的一点是，冷热钱包实际上都不会储存你的资产。他们拿着你的私人钥匙。你的资产总是在区块链。

为了这个演示的目的，我们将把我们的密钥存储在你机器上的一个文件系统中。将此功能添加到您的`01_newAccount.js`和`02_restoreWallet.js`文件: 

```
const { writeFileSync } = require("fs");

function _store(_privateKey, _publicKey, _address) {
  const accountOne = {
    privateKey: _privateKey,
    publicKey: _publicKey,
    address: _address,
}; 
  const accountOneData = JSON.stringify(accountOne);
  writeFileSync("account 2.json", accountOneData);
}
```

## 进行交易

最后，让我们创建发送本地硬币的功能。为此，我们首先需要从文件系统中读取帐户的私钥。然后我们需要创建一个 ethers.js wallet 对象来传递私钥和提供者作为参数。我们将把网络硬编码成一个 Goerli testnet，但是您显然可以扩展它，通过传递本地区块链客户端实例或节点即服务提供者(如 Infura 或 Alchemy)的 URL 来创建您的提供者对象。接下来，我们需要传递接收者的地址和发送的 gETH 数量。最后，我们将创建一个事务对象，并将其广播到网络。

创建一个名为`03_send.js`的新文件，并粘贴以下代码。

```
const { getDefaultProvider, Wallet, utils } = require("ethers");
const { readFileSync } = require("fs");

async function main(_receiverAddress, _ethAmount) {
  const network = "goerli";
  const provider = getDefaultProvider(network);
  const accountRawData = readFileSync("account 1.json", "utf8");
  const accountData = JSON.parse(accountRawData);
  const privateKey = Object.values(accountData.privateKey);
  const signer = new Wallet(privateKey, provider);
  const transaction = await signer.sendTransaction({
    to: _receiverAddress,
    value: utils.parseEther(_ethAmount),
}); 
  console.log(transaction);
}

main(process.argv[2], process.argv[3])
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

要运行它，请键入:

```
node 03_send.js “receiverAddress” “amount”
```

## 总结

您的钱包可以让您读取余额、发送交易以及连接到分散的应用程序。许多钱包也让你从一个应用程序管理几个账户。那是因为钱包没有保管你的资金，你有。它们只是管理真正属于你的东西的工具。

在这篇文章中，你已经学习了如何使用 Node.js、[](https://github.com/ethereum/js-ethereum-cryptography)和[ethers . js](https://docs.ethers.io/v5/getting-started/)开发一个基本的加密钱包。虽然这是一个有趣的工程教程，但强烈建议您使用已经测试和验证过的钱包解决方案，而不是从头开始创建自己的解决方案。

要了解更多信息，请访问 [Chainlink 智能合同范例库](https://github.com/smartcontractkit/smart-contract-examples) ，开始尝试这个和其他范例项目。

访问[chain . link](https://chain.link/)或阅读[docs . chain . link](https://docs.chain.link/)上的文档，了解更多关于 Chainlink 的信息。要讨论集成，请联系专家。