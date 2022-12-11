# 如何建立 Timelock 智能合同

> 原文：<https://blog.chain.link/timelock-smart-contracts/>

本技术教程将教你什么是 timelock 智能合约，以及如何建立它们。您将制定一个智能合约，使 ERC-20 令牌的铸造排队进入一个基于时间的窗口。

本教程将使用:

*   [铸造厂](https://github.com/foundry-rs/foundry)
*   [坚实度](https://docs.soliditylang.org/)
*   [以太坊](https://ethereum.org/)

本教程的代码可以在我们的 [例子 GitHub](https://github.com/smartcontractkit/smart-contract-examples/tree/main/timelocked-contracts) 中找到

## **什么是 Timelock 智能合约？**

从本质上讲，时间锁是一段额外的代码，它将智能合约中的功能限制在特定的时间窗口内。最简单的形式可能类似于这个简单的“如果”语句:

```
if (block.timestamp < _timelockTime) {

    revert ErrorNotReady(block.timestamp, _timelockTime);

} 
```

## **Timelock 合同用例**

时间锁合同有许多潜在的使用案例。它们通常用于首次发行硬币(ico)中，以帮助推动代币的众筹销售。它们还可以用来实现一个授权计划，用户可以在一段时间后提取他们的资金。

另一种可能的用途是作为一种基于智能合同的遗嘱形式。使用链节管理器，你可以定期检查遗嘱的所有者，一旦死亡证明被归档，遗嘱的智能合同可以解锁。

这只是几个例子，但可能性是无穷无尽的。在本例中，我们将重点创建一个[【ERC-20】](https://eips.ethereum.org/EIPS/eip-20)契约，它强制执行一个 timelock 队列来铸造硬币。

## **如何创建 Timelock 智能合同**

在本教程中，我们将使用 Foundry 来构建和测试我们的 Solidity 契约。更多详情可以在 [代工厂 GitHub](https://github.com/foundry-rs/foundry#installation) 上找到。

### **初始化项目**

您将使用 `forge init`初始化项目。 项目初始化后， `forge test` 将进行健全性检查，确保一切设置正确。

```
❯ forge init timelocked-contract

Initializing /Users/rg/Development/timelocked-contract...

Installing ds-test in "/Users/rg/Development/timelocked-contract/lib/ds-test", (url: https://github.com/dapphub/ds-test, tag: None)

    Installed ds-test

    Initialized forge project.

❯ cd timelocked-contract

❯ forge test

[⠒] Compiling...

[⠰] Compiling 3 files with 0.8.10

[⠔] Solc finished in 143.06ms

Compiler run successful

Running 1 test for src/test/Contract.t.sol:ContractTest

[PASS] testExample() (gas: 190)

Test result: ok. 1 passed; 0 failed; finished in 469.71µs
```

### **创建您的测试**

您将创建测试来确保合同满足所有时间锁定的要求。您将检查的主要功能是:

*   排队令牌制造
*   到达窗口后立即打开
*   取消排队造币

除了这些功能之外，你还将确保合同不允许诸如双重排队或不先排队就铸造之类的误用。

一旦项目被初始化，您将能够运行测试。您将使用测试来确保您的项目按预期运行。这些测试属于 `src/test/Contract.t.sol` 。Foundry 使用测试名称来表示他们应该成功还是失败。 `testThisShouldWork` 预计会通过，而如果测试恢复，则 `testFailShouldNotWork` 会通过。这使我们能够测试应该和不应该通过的案例。

这里也有几个约定先解释一下。时间锁基于一个队列，该队列将使用`_toAddress``_amount`和 `time` 参数 的散列。 这些值将使用 `keccak256`进行哈希运算。

```
// Create hash of transaction data for use in the queue

function generateTxnHash(

    address _to,

    uint256 _amount,

    uint256 _timestamp

) public pure returns (bytes32) {

    return keccak256(abi.encode(_to, _amount, _timestamp));

}
```

此外，您需要操纵测试区块链的时间来模拟时间流逝。这是通过 `CheatCodes`代工厂实现的。

```
interface CheatCodes {

    function warp(uint256) external;

}
```

Warp 允许你操作当前块的时间戳。该函数接收一个新时间戳的 uint。我们将用它来为我们当前的时间“添加时间”，模拟时间的流逝。这将允许我们创建一套测试，以确保我们的 timelock 按预期运行。

将 `src/test/Contract.t.sol` 的内容替换为:

```
// SPDX-License-Identifier: UNLICENSED

pragma solidity 0.8.10;

import "ds-test/test.sol";

import "../Contract.sol";

interface CheatCodes {

    function warp(uint256) external;

}

contract ContractTest is DSTest {

    // HEVM_ADDRESS is the pre-defined contract that contains the cheatcodes

    CheatCodes constant cheats = CheatCodes(HEVM_ADDRESS);

    Contract public c;

address toAddr = 0x1234567890123456789012345678901234567890;

    function setUp() public {

        c = new Contract();

        c.queueMint(

            toAddr,

            100,

            block.timestamp + 600

        );

    }

    // Ensure you can't double queue

    function testFailDoubleQueue() public {

        c.queueMint(

            toAddr,

            100,

            block.timestamp + 600

        );

    }

    // Ensure you can't queue in the past

    function testFailPastQueue() public {

        c.queueMint(

            toAddr,

            100,

            block.timestamp - 600

        );

    }

    // Minting should work after the time has passed

    function testMintAfterTen() public {

        uint256 targetTime = block.timestamp + 600;

        cheats.warp(targetTime);

        c.executeMint(

            toAddr,

            100,

            targetTime

        );

    }

    // Minting should fail if you mint too soon

    function testFailMintNow() public {

        c.executeMint(

            toAddr,

            100,

            block.timestamp + 600

        );

    }

    // Minting should fail if you didn't queue

    function testFailMintNonQueued() public {

        c.executeMint(

            toAddr,

            999,

            block.timestamp + 600

        );

    }

    // Minting should fail if try to mint twice

    function testFailDoubleMint() public {

        uint256 targetTime = block.timestamp + 600;

        cheats.warp(block.timestamp + 600);

        c.executeMint(

            toAddr,

            100,

            targetTime

        );

        c.executeMint(

            toAddr,

            100,

            block.timestamp + 600

        );

    }

    // Minting should fail if you try to mint too late

    function testFailLateMint() public {

        uint256 targetTime = block.timestamp + 600;

        cheats.warp(block.timestamp + 600 + 1801);

        emit log_uint(block.timestamp);

        c.executeMint(

            toAddr,

            100,

            targetTime

        );

    }

    // you should be able to cancel a mint

    function testCancelMint() public {

        bytes32 txnHash = c.generateTxnHash(

            toAddr,

            100,

            block.timestamp + 600

        );

        c.cancelMint(txnHash);

    }

    // you should be able to cancel a mint once but not twice

    function testFailCancelMint() public {

        bytes32 txnHash = c.generateTxnHash(

            toAddr,

            999,

            block.timestamp + 600

        );

        c.cancelMint(txnHash);

        c.cancelMint(txnHash);

    }

    // you shouldn't be able to cancel a mint that doesn't exist

    function testFailCancelMintNonQueued() public {

        bytes32 txnHash = c.generateTxnHash(

            toAddr,

            999,

            block.timestamp + 600

        );

        c.cancelMint(txnHash);

    }

}
```

### **建立合同**

你现在应该可以运行 `forge test` 并看到许多错误。现在是时候让你的测试通过了。

我们将从基本的 ERC-20 合同开始。所有这些工作都属于 `src/Contract.sol`。

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.10;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

import "@openzeppelin/contracts/access/Ownable.sol";

contract Contract is ERC20, Ownable {

    constructor() ERC20("TimeLock Token", "TLT") {}

}
```

要使用 OpenZeppelin 合同，您需要安装它们，并将 Foundry 指向它们。

要安装合同，运行

```
❯ forge install openzeppelin/openzeppelin-contracts
```

您还需要通过创建 `remappings.txt`来映射导入。

```
@openzeppelin/=lib/openzeppelin-contracts/

ds-test/=lib/ds-test/src/
```

这个重映射文件允许你使用像 OpenZeppelin 契约这样的东西，并以你通常使用其他工具如 Hardhat 或 Remix 的方式导入它们。该文件将导入重新映射到它们所在的目录。我还通过 `forge install openzeppelin/openzeppelin-contracts`安装了 OpenZeppelin 契约。 这些将用于创建 ERC-721 合同。

如果一切正常，您可以运行 `forge build` 来编译合同。

此时，您可以构建下面的合同。该合同将允许您排队造币，并在适当的窗口期间返回执行该造币。

```
// SPDX-License-Identifier: UNLICENSED

pragma solidity 0.8.10;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

import "@openzeppelin/contracts/access/Ownable.sol";

contract Contract is ERC20, Ownable {

    // Error Messages for the contract

    error ErrorAlreadyQueued(bytes32 txnHash);

    error ErrorNotQueued(bytes32 txnHash);

    error ErrorTimeNotInRange(uint256 blockTimestmap, uint256 timestamp);

    error ErrorNotReady(uint256 blockTimestmap, uint256 timestamp);

    error ErrorTimeExpired(uint256 blockTimestamp, uint256 expiresAt);

    // Queue Minting Event

    event QueueMint(

        bytes32 indexed txnHash,

        address indexed to,

        uint256 amount,

        uint256 timestamp

    );

    // Mint Event

    event ExecuteMint(

        bytes32 indexed txnHash,

        address indexed to,

        uint256 amount,

        uint256 timestamp

    );

    // Cancel Mint Event

    event CancelMint(bytes32 indexed txnHash);

    // Constants for minting window

    uint256 public constant MIN_DELAY = 60; // 1 minute

    uint256 public constant MAX_DELAY = 3600; // 1 hour

    uint256 public constant GRACE_PERIOD = 1800; // 30 minutes

    // Minting Queue

    mapping(bytes32 => bool) public mintQueue;

    constructor() ERC20("TimeLock Token", "TLT") {}

    // Create hash of transaction data for use in the queue

    function generateTxnHash(

        address _to,

        uint256 _amount,

        uint256 _timestamp

) public pure returns (bytes32) {

        return keccak256(abi.encode(_to, _amount, _timestamp));

    }

    // Queue a mint for a given address amount, and timestamp

    function queueMint(

        address _to,

        uint256 _amount,

        uint256 _timestamp

) public onlyOwner {

        // Generate the transaction hash

        bytes32 txnHash = generateTxnHash(_to, _amount, _timestamp);

        // Check if the transaction is already in the queue

        if (mintQueue[txnHash]) {

            revert ErrorAlreadyQueued(txnHash);

        }

        // Check if the time is in the range

        if (

            _timestamp < block.timestamp + MIN_DELAY ||

            _timestamp > block.timestamp + MAX_DELAY

        ) {

            revert ErrorTimeNotInRange(_timestamp, block.timestamp);

        }

        // Queue the transaction

        mintQueue[txnHash] = true;

        // Emit the QueueMint event

        emit QueueMint(txnHash, _to, _amount, _timestamp);

    }

    // Execute a mint for a given address, amount, and timestamp

    function executeMint(

        address _to,

        uint256 _amount,

        uint256 _timestamp

) external onlyOwner {

        // Generate the transaction hash

        bytes32 txnHash = generateTxnHash(_to, _amount, _timestamp);

        // Check if the transaction is in the queue

        if (!mintQueue[txnHash]) {

            revert ErrorNotQueued(txnHash);

        }

        // Check if the time has passed

        if (block.timestamp < _timestamp) {

            revert ErrorNotReady(block.timestamp, _timestamp);

        }

        // Check if the window has expired

        if (block.timestamp > _timestamp + GRACE_PERIOD) {

            revert ErrorTimeExpired(block.timestamp, _timestamp);

        }

        // Remove the transaction from the queue

        mintQueue[txnHash] = false;

        // Execute the mint

        mint(_to, _amount);

        // Emit the ExecuteMint event

        emit ExecuteMint(txnHash, _to, _amount, _timestamp);

    }

    // Cancel a mint for a given transaction hash

    function cancelMint(bytes32 _txnHash) external onlyOwner {

        // Check if the transaction is in the queue

        if (!mintQueue[_txnHash]) {

            revert ErrorNotQueued(_txnHash);

        }

        // Remove the transaction from the queue

        mintQueue[_txnHash] = false;

        // Emit the CancelMint event

        emit CancelMint(_txnHash);

    }

    // Mint tokens to a given address

    function mint(address to, uint256 amount) internal {

        _mint(to, amount);

    }

}
```

## **何去何从**

时间锁合同本身就很强大。它们为智能合约中的交易提供了安全措施和透明度。不过，它们不是独立工作的。您将需要回来并在适当的窗口内执行功能，除非您自动化您的合同。

[链节保持器](https://chain.link/keepers%20) 允许您在智能合约中自动调用函数。这将使您能够创建在预定义窗口内自动执行的排队函数，消除错过执行窗口的风险。要了解更多信息，请前往 [保管文档](https://docs.chain.link/docs/chainlink-keepers/job-scheduler/%20)

如果您是一名开发人员，并且想要将 Chainlink 集成到您的智能合同应用程序中，请查看 [【区块链教育中心】](https://blockchain.education/) ， [开发人员文档](https://docs.chain.link/docs) 或 [联系专家](https://chainlink.typeform.com/to/gEwrPO) 。你也可以 [通过分散的神谕将你的智能合约与现实世界的数据联系起来。](https://docs.chain.link/docs/conceptual-overview/)