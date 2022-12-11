# 如何建立区块链彩票

> 原文：<https://blog.chain.link/how-to-build-a-blockchain-lottery/>

## 为什么要做去中心化的彩票？

建立一个区块链彩票(或分散型彩票)只需要几份合同的代码，并且相对容易运作。使用 [Chainlink 可验证随机函数(VRF)](https://chain.link/solutions/chainlink-vrf) 和 [Chainlink 闹钟](https://docs.chain.link/docs/chainlink-alarm-clock)的开发者有一个易于维护的彩票合同，它是安全的、永久的、可证明随机的。然而，在我们进入*如何*建立分散式彩票之前，让我们快速了解一下*为什么*我们应该建立区块链彩票。

### 可证明是随机的/可靠的

目前的彩票要求你相信无论谁在经营彩票，他都会诚实地经营。可悲的是，情况并非总是如此。最近，发生了一起[彩票被操纵超过 1400 万美元](https://wjla.com/newschannel-8/ex-lottery-worker-convicted-of-rigging-hot-lotto-system-to-win-14m-115718)的事件。这不应该发生；一个玩家永远不应该担心经营彩票的人有作弊的能力。区块链彩票可以通过提供一个不能被破坏或黑客攻击的平台来解决这个问题，在这个平台上，号码是随机选择的，每个人都可以*证明*他们是随机选择的。

这无疑是为什么彩票在区块链会好得多的最重要的原因。任何彩票的一个重要部分是每个参与者对他们的钱被公平对待的信任。

### 降低开销

对于所有集中式应用程序，开销是一场噩梦。州彩票目前必须支付:

1.  维护彩票服务器的工作人员
2.  维护门票和包装的工作人员
3.  电视广告、广播和网络广告
4.  员工创造新的游戏

就目前情况来看，区块链彩票不能解决所有这些问题，但至少可以解决第一和第四个问题。一旦合同已经签订并被证明是安全的，你就再也不用重新发明轮子了。你可以在同一个开源彩票[智能合约](https://chain.link/education/smart-contracts)上放置一个前端。服务器现在完全被您正在运行的区块链所取代。

## 我们如何建立一个基于区块链的去中心化彩票？

为了简单起见，我们将跟随这个 [Chainlink 彩票](https://github.com/alphachainio/chainlink-lottery/tree/master/ethereum/contracts) github 回购的代码。有很多这样的例子，包括[糖果店](https://github.com/itsthecandyshop/contracts/tree/master/contracts)(ETH 全球 HackMoney 赢家)[乐透水牛](https://github.com/GMSteuart/lotto-buffalo/tree/master/ethereum/contracts)(科罗拉多州 GameJam 赢家)，以及[水牛的 WoF](https://github.com/elviric/BWoF/blob/master/smartcontract/BWoF.sol) (科罗拉多州 GameJam 赢家)。

在这个演练中，我们将使用一个单一的神谕来刺激我们的彩票。这将使彩票集中在这些单个节点上。理想情况下，你会想多走一步，设置或连接一个网络到几个闹钟和 Chainlink VRFs，这样彩票就真正分散了，但在设置好初始部分后，这应该是相当微不足道的。

本教程中使用的代码未经审计或审查，纯粹用于教学目的。在尝试经营彩票之前，也请检查当地法律和政府法规，无论是否分散，因为彩票是高度管制的。

### 设置

为了简单起见，我们将像您使用 [Remix](https://remix.ethereum.org/#version=soljson-v0.6.6+commit.6c089d02.js&optimize=false&gist=1bb62664cfad04de04d9dc5d059eb519&evmVersion=null) 一样进行演示。您可以使用该链接来部署本演练中使用的所有代码。然而，我们建议学习[松露](https://www.trufflesuite.com/) / [Buidler](https://buidler.dev/) 和 [React](https://reactjs.org/docs/create-a-new-react-app.html) 来拥有一个完整的开发套件/应用。使用 Truffle/Buidler 和 React，您可以快速迭代测试、部署，并向您的应用程序添加前端。要深入了解如何在智能合约中使用 Truffle 进行开发，一定要查看这个教程博客[。您会注意到，Nodejs 应用程序上的导入语法与 Remix 中的略有不同，但除此之外，下面的所有代码都是一样的。你也可以从上面克隆或派生任何一个库来自己使用它们。](https://www.trufflesuite.com/blog/using-truffle-to-interact-with-chainlink-smart-contracts)

### 让我们开始建造吧

从概念上讲，在抽奖中我们只需要做几件事情。

1.  参加抽奖并跟踪参赛作品
2.  在某个时间随机挑选赢家
3.  管理支出
4.  重复

为了开始第一部分，我们创建了“Lottery.sol”契约。我们希望创建一些实例变量来跟踪玩家、彩票的状态，以及可能到目前为止我们已经玩了多少次彩票(' lotteryId ')。我们当然希望我们的契约继承 ChainlinkClient，这样我们就可以利用 Chainlink 警报。

您会注意到，我们现在忽略了接口和治理部分。我们以后再谈这个。

```
pragma solidity ^0.6.6;
import "github.com/smartcontractkit/chainlink/evm-contracts/src/v0.6/ChainlinkClient.sol";

contract Lottery is ChainlinkClient {
    enum LOTTERY_STATE { OPEN, CLOSED, CALCULATING_WINNER }
    LOTTERY_STATE public lottery_state;
    address payable[] public players;
    uint256 public lotteryId;
}
```

接下来，我们将需要我们的构造函数，在那里我们可以设置一些初始变量。我们将彩票的状态设置为 closed(因为它还没有开始)，并将“lotteryId”设置为“1”。我们还需要“setPublicChainlinkToken()”来与 Chainlink oracles 交互。

```
constructor() public {
    setPublicChainlinkToken();
    lotteryId = 1;
    lottery_state = LOTTERY_STATE.CLOSED;
}
```

## 为抽奖设置一个计时器

太好了，那我们怎么开始抽奖呢？我们如何让彩票在我们希望的时候开始和结束？这就是链式报警的用武之地，我们可以创建一个连接到链式报警的功能，当时间到了，它将停止/关闭抽奖。

我们需要指定' CHAINLINK_ALARM_JOB_ID '和' CHAINLINK_ALARM_ORACLE '以及支持 CHAINLINK 警报的 CHAINLINK 节点。你可以在文档中找到它们[。此功能告诉闹钟在“现在+持续时间”之后返回“完成 _ 报警功能”。持续时间是您希望闹钟等待的时间，以秒为单位。当持续时间过去后，报警将调用“履行 _ 报警”。](https://docs.chain.link/docs/chainlink-alarm-clock)

接下来，我们希望‘fulfill _ alarm’能够选出获胜者。我们通过添加‘pick winner’函数来实现这一点，我们将在后面进行描述。

```
function start_new_lottery(uint256 duration) public {
    require(lottery_state == LOTTERY_STATE.CLOSED, "can't start a new lottery yet");
    lottery_state = LOTTERY_STATE.OPEN;
    Chainlink.Request memory req = buildChainlinkRequest(CHAINLINK_ALARM_JOB_ID, address(this), this.fulfill_alarm.selector);
    req.addUint("until", now + duration);
    sendChainlinkRequestTo(CHAINLINK_ALARM_ORACLE, req, ORACLE_PAYMENT);
}

function fulfill_alarm(bytes32 _requestId)
    public
    recordChainlinkFulfillment(_requestId)
{
    require(lottery_state == LOTTERY_STATE.OPEN, "The lottery hasn't even started!");
    lottery_state = LOTTERY_STATE.CALCULATING_WINNER;
    lotteryId = lotteryId + 1;
    pickWinner();
}
```

## 参加抽奖

现在我们知道如何开始抽奖，玩家需要能够通过在 ETH 中购买门票来进入抽奖。你可以设定一个“最低”票价，或者只是设定一个价格，这样用户就必须送出一定数量的 ETH 才能参加抽奖。

下面的代码检查彩票是否公开，用户是否输入了获得彩票的最低金额。然后，它将用户推送到已输入用户的动态数组中。

```
function enter() public payable {
    assert(msg.value == MINIMUM);
    assert(lottery_state == LOTTERY_STATE.OPEN);
    players.push(msg.sender);
}
```

这就是我们对于用户管理所要做的一切。现在我们进入有趣的部分，选出获胜者。

## 挑选获胜者

在我们的演示中，我们将有一个不同的契约句柄使用 VRF 链绘制一个随机数。我们将不得不为我们的所有契约构建接口，以便它们可以轻松地相互交互，然后制定一个将它们相互连接的治理契约。(你也可以让它们互相治理，但是如果你决定你的彩票需要很多特性和很多智能契约，那么很容易把它们都放在一个治理契约中)。

“pickWinner”函数看起来会像这样:

```
function pickWinner() private {
    require(lottery_state == LOTTERY_STATE.CALCULATING_WINNER, "You aren't at that stage yet!");
    RandomnessInterface(governance.randomness()).getRandom(lotteryId, lotteryId);
    //this kicks off the request and returns through fulfill_random
}
```

在这个函数中，我们说:

“给我‘governance . randomnes()’函数中定义的随机性智能合约的地址。随机性智能契约将有一个“getRandom”函数，它将两个 uint256 作为参数。"

## 将彩票和随机性联系起来

我们希望定义一个治理智能契约来连接我们的契约，该契约将处理我们的彩票契约获取随机数。我们的治理契约将看起来像这样:

```
pragma solidity ^0.6.6;

contract Governance {
    uint256 public one_time;
    address public lottery;
    address public randomness;
    constructor() public {
    }

    function init(address _lottery, address _randomness) public {
        require(_randomness != address(0), "governance/no-randomnesss-address");
        require(_lottery != address(0), "no-lottery-address-given");
        randomness = _randomness;
        lottery = _lottery;
    }
}
```

每次我们部署彩票契约和随机契约时，都应该调用 init 函数，以便它们都知道另一个契约的地址。

界面看起来会非常简单:

```
pragma solidity 0.6.6;

interface RandomnessInterface {
    function randomNumber(uint) external view returns (uint);
    function getRandom(uint, uint) external;
}
```

该接口将是需要与另一个智能合同共享的任何少数功能。要更深入地了解为什么需要接口，请务必查看这个对[接口与抽象契约](https://medium.com/upstate-interactive/solidity-how-to-know-when-to-use-abstract-contracts-vs-interfaces-874cab860c56)的深入探究。你可以在 [Remix](https://remix.ethereum.org/#version=soljson-v0.6.6+commit.6c089d02.js&optimize=false&gist=1bb62664cfad04de04d9dc5d059eb519&evmVersion=null) 中看到接口的样本设置，如果你想看它们的运行。

一旦我们将它们与治理契约和接口连接起来，我们就可以开始考虑获得一个随机数。

## 得到一个随机数

获得一个随机数几乎与 [Chainlink VRF 文档](https://docs.chain.link/docs/chainlink-vrf)中的演示代码相同。我们正在 Ropsten 上测试它，所以我们几乎可以用文档中的精确代码运行。

```
/**
  * Constructor inherits VRFConsumerBase
  * 
  * Network: Ropsten
  * Chainlink VRF Coordinator address: 0xf720CF1B963e0e7bE9F58fd471EFa67e7bF00cfb
  * LINK token address:                0x20fE562d797A42Dcb3399062AE9546cd06f63280
  * Key Hash: 0xced103054e349b8dfb51352f0f8fa9b5d20dde3d06f9f43cb2b85bc64b238205
  */
constructor(address _governance) 
    VRFConsumerBase(
        0xf720CF1B963e0e7bE9F58fd471EFa67e7bF00cfb, // VRF Coordinator
        0x20fE562d797A42Dcb3399062AE9546cd06f63280  // LINK Token
    ) 
    public
{
    keyHash = 0xced103054e349b8dfb51352f0f8fa9b5d20dde3d06f9f43cb2b85bc64b238205;
    fee = 0.1 * 10 ** 18; // 0.1 LINK
    governance = GovernanceInterface(_governance);
}
```

在构造函数中，我们将添加一个关于连接到治理契约的部分。唯一不同的是在“fulfillRandomness”函数中，我们将把得到的随机数传递回彩票智能合约。

```
/**
  * Callback function used by VRF Coordinator
  */
function fulfillRandomness(bytes32 requestId, uint256 randomness) external override {
    require(msg.sender == vrfCoordinator, "Fulfillment only permitted by Coordinator");
    most_recent_random = randomness;
    uint lotteryId = requestIds[requestId];
    randomNumber[lotteryId] = randomness;
    LotteryInterface(governance.lottery()).fulfill_random(randomness);
}
```

## 挑选获胜者

我们调用彩票合同的‘fulfill _ random’来根据随机数挑选赢家。我们使用一个简单的模数来得到随机数。所以回到彩票合同，我们会有:

```
function fulfill_random(uint256 randomness) external {
    require(lottery_state == LOTTERY_STATE.CALCULATING_WINNER, "You aren't at that stage yet!");
    require(randomness > 0, "random-not-found");
    uint256 index = randomness % players.length;
    players[index].transfer(address(this).balance);
    players = new address payable[](0);
    lottery_state = LOTTERY_STATE.CLOSED;
}
```

这将结束抽奖。然后，您可以通过调用“start_new_lottery”函数来实现此合同循环，只要您的合同始终由 LINK 提供资金来支付 oracle gas 成本，您的彩票就可以永远运行下去。

## 今天就开始用 VRF 链家大厦吧

如果你在这里学到了一些新东西，想要展示你已经建立的东西，为一些演示回购开发了一个前端，甚至用 PR 改进了任何回购，请确保你在 [Twitter](https://twitter.com/chainlink) 、 [Discord](https://discord.gg/Szt3FYj) 或 [Reddit](https://www.reddit.com/r/Chainlink/) 上分享它，并用#chainlink 和#ChainlinkVRF 标记你的回购。

如果你正在开发一个可以从 Chainlink oracles 中受益的产品，或者想要帮助 Chainlink 网络的开源开发，请访问[开发者文档](https://docs.chain.link/)或者加入关于 [Discord](https://discordapp.com/invite/aSK4zew) 的技术讨论。

*[chain link 2021 秋季黑客马拉松](https://chain.link/hackathon) 于 2021 年 10 月 22 日开赛。无论您是开发人员、创作人员、艺术家、区块链专家，还是该领域的新手，这个黑客马拉松都是启动您的智能合同开发之旅并向行业领先的导师学习的最佳场所。立即锁定您的席位，争夺超过$ 300，000 的奖金。T9】*

[Sign up today](https://chain.link/hackathon?utm_medium=referral&utm_source=chainlink-blog&utm_campaign=fall-2021-hackathon&utm_content=how-to-build-a-blockchain-lottery)