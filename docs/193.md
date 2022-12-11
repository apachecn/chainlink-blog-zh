# 如何创建一个加密硬币

> 原文：<https://blog.chain.link/how-to-create-a-crypto-coin/>

货币是商品和服务的交换媒介。如今，货币采取纸张、硬币或集中数字分类账条目的形式，通常由政府发行，并被普遍接受为支付方式。在过去，货币采取各种金属的形式，如金或银，甚至彩色珠子和盐。加密货币是一种以密码学和区块链技术为基础的数字货币形式，主要用于转移价值，而不必依赖于单一的中央平台，如银行。

在本技术教程中，我们将探索硬币和代币之间的区别，您将学习如何开发自己的加密硬币。

我们开始吧！

## 硬币和代币的区别

比特币是最受欢迎的加密货币，其主要目的是作为交换媒介。还有许多有价值的令牌，但是除了作为一种交换形式的效用之外，还具有额外的目的:例如，治理投票令牌授予持有者某些治理特权。还有 NFT:不可替换的令牌，代表某个独特事物的所有权。那么，所有这些类型的数字资产之间有什么区别呢？

从工程的角度来看，硬币和代币的区别非常简单。硬币是单一区块链的一部分，而代币以智能合约的形式在现有的区块链上运行。

例如，BTC 是比特币区块链的硬币，而 ETH 是以太坊区块链的硬币。BTC 和 ETH 都是硬币。仅举几个例子，USDC、AAVE 和 WETH 就是代币，因为它们本质上是托管在以太坊区块链之上的智能合约。同样的原则也适用于 NFT:它们也是驻留在各种通用区块链上的令牌。

要了解如何创建自己的令牌，请查看这篇博文 [。](https://blog.chain.link/how-to-create-an-erc-20-token-on-polygon/) 要了解更多关于创建 NFT 的信息，请查看本指南 。继续阅读，了解更多关于创建自己的加密硬币。

## 入门

这个项目是用 Go 编写的，但是不需要以前有这种语言的经验。要跟进，请查看位于我的加密硬币文件夹下的 [Chainlink 智能合同范例库](https://github.com/smartcontractkit/smart-contract-examples) 中的完整工作范例。

```
git clone https://github.com/smartcontractkit/smart-contract-examples.git
cd smart-contract-examples/my-crypto-coin
```

下一步是在你的本地机器上安装 Go，你可以按照官方指南来做。由于这一过程需要大约 10 分钟，所以在完成时你有时间煮咖啡。

继续之前，请确认您的`$GOPATH`设置正确。这是一个强制步骤。

惯例是将源代码存储在`$GOPATH/src`中，将编译后的程序二进制文件存储在`$GOPATH/bin`中。导航到`$GOPATH/src`并创建一个名为`my-crypto-coin`的新文件夹。

先说发展。

## 一切从创世纪区块开始

硬币是区块链分布式账本中的单位。每个区块链都有它的初始状态，也被称为创世板块。在新创建的`my-crypto-coin`项目中，创建一个新文件夹，并将其命名为`ledger`。在`ledger`文件夹中，创建一个新文件，命名为`genesis.json`，并粘贴下面的代码。我们将向爱丽丝分配最初 100 万枚加密硬币。

```
{
 "genesis_time": "2022-04-12T00:00:00.000000000Z",
 "chain_id": "our-blockchain",
 "balances": {
 "alice": 1000000
 }
}
```

这是原始状态。事务改变状态。如果我们的区块链节点出现故障，我们可以使用源文件和交易历史来重新创建整个分类帐，并将网络的其余部分同步到最新状态。

## 账户、交易和全球状态

导航到`ledger`文件夹并创建一个`tx.go`文件。每个账户将由账户结构表示。每个事务都将由事务结构表示，具有以下属性:“from”、“to”和“value”。我们将添加创建新账户和交易的功能。

```
package ledger

type Account string

type Tx struct {
 From  Account `json:"from"`
 To    Account `json:"to"`
 Value uint    `json:"value"`
}

func NewAccount(value string) Account {
 return Account(value)
}

func NewTx(from Account, to Account, value uint) Tx {
 return Tx{from, to, value}
}
```

交易将存储在分类账中，所以让我们手动添加几个交易作为演示。在`ledger`目录中，创建一个新的`ledger.db`文件，并将以下内容粘贴到那里。

```
{"from":"alice","to":"bob","value":10}
{"from":"alice","to":"alice","value":3}
{"from":"alice","to":"alice","value":500}
{"from":"alice","to":"bob","value":100}
{"from":"bob","to":"alice","value":5}
{"from":"bob","to":"carol","value":10}
```

创世状态保持不变，并保留在`genesis.json`文件中。让我们添加一种以编程方式加载其状态的方法。创建一个名为`genesis.go`的新文件，该文件将存储一个帐户映射，其中包含创世状态下相应的硬币余额。

```
package ledger

import (
 "io/ioutil"
 "encoding/json"
)

type Genesis struct {
 Balances map[Account]uint `json:"balances"`
}

func loadGenesis(path string) (Genesis, error) {
 genesisFileContent, err := ioutil.ReadFile(path)
 if err != nil {
 return Genesis{}, err
 }

 var loadedGenesis Genesis

 err = json.Unmarshal(genesisFileContent, &loadedGenesis)
 if err != nil {
 return Genesis{}, err
 }

return loadedGenesis, nil
} 
```

核心业务逻辑将存储在商店结构中。创建一个名为`state.go`的新文件。状态结构将包含所有帐户余额的详细信息，谁将硬币转移给了谁，以及转移了多少硬币。它必须知道如何从源文件中读取初始状态。之后，通过顺序地 重放来自`ledger.db`文件的所有事务来更新起源状态余额。最后，这里我们需要编写一个向分类帐添加新交易的逻辑。

```
package ledger

import (
 "fmt"
 "os"
 "path/filepath"
 "bufio"
 "encoding/json"
)

type State struct {
 Balances  map[Account]uint
 txMempool []Tx

 dbFile *os.File
}

func SyncState() (*State, error)  {
 cwd, err := os.Getwd()
 if err != nil {
 return nil, err
 }

 gen, err := loadGenesis(filepath.Join(cwd, "ledger", "genesis.json"))
 if err != nil {
 return nil, err
 }

 balances := make(map[Account]uint)
 for account, balance := range gen.Balances {
 balances[account] = balance
 }

 file, err := os.OpenFile(filepath.Join(cwd, "ledger", "ledger.db"), 
os.O_APPEND|os.O_RDWR, 0600)
 if err != nil {
 return nil, err
 }

 scanner := bufio.NewScanner(file)

 state := &State{balances, make([]Tx, 0), file}

 for scanner.Scan() {
 if err := scanner.Err(); err != nil {
 return nil, err
 }

 var transaction Tx
 json.Unmarshal(scanner.Bytes(), &transaction)

 if err := state.writeTransaction(transaction); err != nil {
 return nil, err
 }
 }

 return state, nil
}

func (s *State) writeTransaction(tx Tx) error {
 if s.Balances[tx.From] < tx.Value {
 return fmt.Errorf("insufficient balance")
 }

 s.Balances[tx.From] -= tx.Value
 s.Balances[tx.To] += tx.Value

 return nil
}

func (s *State) Close() {
 s.dbFile.Close()
}

func (s *State) WriteToLedger(tx Tx) error {
 if err := s.writeTransaction(tx); err != nil {
 return err
 }

 s.txMempool = append(s.txMempool, tx)

 mempool := make([]Tx, len(s.txMempool))
 copy(mempool, s.txMempool)

 for i := 0; i < len(mempool); i++ {
 txJson, err := json.Marshal(mempool[i])
 if err != nil {
 return err
 }

 if _, err = s.dbFile.Write(append(txJson, '\n')); err != nil {
 return err
 }
 s.txMempool = s.txMempool[1:]
 }

 return nil
}
```

## 开发命令行界面(CLI)

使用我们新的加密硬币最简单的方法是开发命令行界面(CLI)。在 Go 中开发基于 CLI 的程序最简单的方法就是使用 [下一个第三方库](https://github.com/spf13/cobra) 。要使用这个库，我们需要为我们的项目初始化 Go 的内置依赖管理器，称为 Go 模块。Go 模块命令将自动获取你在 Go 文件中引用的任何库。

```
echo $GOPATH
cd $GOPATH/src/my-crypto-coin 
go mod init
```

现在让我们创建一个新文件夹，命名为`cli`。但是等等，我们还没有正式命名我们的加密硬币！暂且称之为我的加密硬币吧。导航到`cli`文件夹，创建一个名为`mcc`的新文件夹，这是我的加密硬币的简称。导航到`mcc`文件夹。

创建一个名为`main.go`的新文件。这将是我们节目的主入口。

```
package main

import (
 "github.com/spf13/cobra"
 "os"
 "fmt"
)

func main() {
 var mccCmd = &cobra.Command{
 Use:   "mcc",
 Short: "My Crypto Coin CLI",
 Run: func(cmd *cobra.Command, args []string) {
 },
 }

 mccCmd.AddCommand(versionCmd)
 mccCmd.AddCommand(balancesCmd())
 mccCmd.AddCommand(txCmd())

 err := mccCmd.Execute()
 if err != nil {
 fmt.Fprintln(os.Stderr, err)
 os.Exit(1)
 }
}
```

接下来，创建一个`version.go`文件，粘贴这个内容。

```
package main

import (
 "fmt"
 "github.com/spf13/cobra"
)

const Major = "0"
const Minor = "1"
const Fix = "0"
const Verbal = "Genesis"

var versionCmd = &cobra.Command{
 Use:   "version",
 Short: "Describes version.",
 Run: func(cmd *cobra.Command, args []string) {
 fmt.Println(fmt.Sprintf("Version: %s.%s.%s-beta %s", Major, Minor, Fix, Verbal))
 },
}
```

之后，让我们创建一个从分类账中读取所有账户余额的机制。创建一个新的`balances.go`文件。

```
package main

import (
 "github.com/spf13/cobra"
 "my-crypto-coin/ledger"
 "fmt"
 "os"
)

func balancesCmd() *cobra.Command {
 var balancesCmd = &cobra.Command{
 Use:   "balances",
 Short: "Interact with balances (list...).",
 Run: func(cmd *cobra.Command, args []string) {
 },
 }

 balancesCmd.AddCommand(balancesListCmd)

 return balancesCmd
}

var balancesListCmd = &cobra.Command{
 Use:   "list",
 Short: "Lists all balances.",
 Run: func(cmd *cobra.Command, args []string) {
 state, err := ledger.SyncState()
 if err != nil {
 fmt.Fprintln(os.Stderr, err)
 os.Exit(1)
 }
 defer state.Close()

 fmt.Println("Accounts balances:")
 fmt.Println("__________________")
 fmt.Println("")
 for account, balance := range state.Balances {
 fmt.Println(fmt.Sprintf("%s: %d", account, balance))
 }
 },
}
```

最后，让我们添加一个将交易写入分类帐的命令。创建新的`tx.go`文件。

```
package main

import (
 "github.com/spf13/cobra"
 "my-crypto-coin/ledger"
 "fmt"
 "os"
)

func txCmd() *cobra.Command {
 var txsCmd = &cobra.Command{
 Use:   "tx",
 Short: "Interact with transactions (new...).",
 Run: func(cmd *cobra.Command, args []string) {
 },
 }

 txsCmd.AddCommand(newTxCmd())

 return txsCmd }

func newTxCmd() *cobra.Command {
 var cmd = &cobra.Command{
 Use:   "new",
 Short: "Adds new TX to the ledger.",
 Run: func(cmd *cobra.Command, args []string) {
 from, _ := cmd.Flags().GetString("from")
 to, _ := cmd.Flags().GetString("to")
 value, _ := cmd.Flags().GetUint("value")

 tx := ledger.NewTx(ledger.NewAccount(from), ledger.NewAccount(to), 
value)
 state, err := ledger.SyncState()
 if err != nil {
 fmt.Fprintln(os.Stderr, err)
 os.Exit(1)
 }
 defer state.Close()
 err = state.WriteToLedger(tx)
 if err != nil {
 fmt.Fprintln(os.Stderr, err)
 os.Exit(1)
 }

 fmt.Println("TX successfully added to the ledger.")
 }, }

 cmd.Flags().String("from", "", "From what account to send coins")
 cmd.MarkFlagRequired("from")

 cmd.Flags().String("to", "", "To what account to send coins")
 cmd.MarkFlagRequired("to")

 cmd.Flags().Uint("value", 0, "How many coins to send")
 cmd.MarkFlagRequired("value")

 return cmd
} 
```

使用以下命令编译程序:

```
go install $GOPATH/src/my-crypto-coin/cli/mcc/… 
```

Go 会检测缺失的库，并在编译程序前自动获取它们。根据您的`$GOPATH`，生成的程序将保存在`$GOPATH/bin`文件夹中。

要验证安装是否成功，请运行以下命令:

```
which mcc
```

您应该会在终端中看到一个类似于这个路径的路径:`/Users/yourname/go/bin/mcc`。 让我们看看所有可用的命令。运行:  

```
mcc --help
```

要查看 CLI 的当前版本，请运行:

```
mcc version
```

要查看当前用户余额，请运行以下命令:

```
mcc balances list
```

输出应该是:

```
Accounts balances:
__________________

alice: 999895
bob: 95
carol: 10
```

现在让我们使用 CLI 进行第一笔交易。输入以下命令:

```
mcc tx new --from alice --to carol --value 10
```

如果你打开`ledger/ledger.db`文件，你应该能看到另外一行:

```
{"from":"alice","to":"carol","value":10}
```

让我们使用`mcc balances list`命令再次列出余额。输出应该是:

```
Accounts balances:
__________________

alice: 999885
bob: 95
carol: 20
```

## 下一步是什么？

目前，我们的用户在分类帐中以他们的名字表示。但是如果有两个 bob 会怎么样呢？我们需要添加一种方法，使用通用的哈希算法来唯一地表示帐户。

下一个问题是，任何人都可以使用我们的 CLI 转移其他人的硬币。我们需要一种方法，允许用户使用公钥加密技术只转移他们拥有的硬币。

如果我们的机器坏了，会发生什么？没有办法重建我们的网络，因为我们是网络中唯一的节点。我们需要激励人们加入我们的网络，成为节点。一旦我们的网络增长，我们将需要一种通用的方法来确定哪些交易是有效的，哪些是无效的，并验证网络的当前状态。我们需要一个一致的算法和机制。

## 总结

在这篇文章中，你已经学习了如何使用 Go 开发一个基本的加密硬币，并且我们已经讨论了硬币和代币之间的主要区别。要了解更多信息，请访问 [Chainlink 智能合同示例库](https://github.com/smartcontractkit/smart-contract-examples) ，开始尝试这个和其他示例项目。    访问[chain . link](https://chain.link/)或阅读[docs . chain . link](https://docs.chain.link/)上的文档，了解更多关于 Chainlink 的信息。要讨论集成，请联系专家。T29】