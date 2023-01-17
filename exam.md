# 区块链培训作业

请大家 fork 本仓库后答题，做完后提交自己的软件仓库链接。

## 第 1 题：Solidity 语言有哪些数据类型？

例如：

- bool
- int8

评分标准：每个数据类型计 1 分  
参考资料： <https://docs.soliditylang.org/en/latest/types.html>

## 答

- bool、int8、int256、uint256、fixed 、 ufixed、address、bytes、bytes32、bytes1-32、string、enum、数组

## 第 2 题：列举并测试以太坊的 JSONRPC API

评分标准：每条有效的（提供文本命令和测试截图） API 计 2 分，例如：

---

第 1 个 API： net_version

```shell
curl -s -X POST -H "Content-Type: application/json" https://matic-mumbai.chainstacklabs.com -d '{"id":1,"jsonrpc":"2.0","method":"net_version","params":[]}' | jq
```

![1673317288973](https://user-images.githubusercontent.com/7695325/211447294-e9e142c1-0fec-4588-9c8a-7ebfbd38a907.png)

---

## 答

- eth_blockNumber：返回最新块的编号。

    ```shell
    curl -s -X POST -H "Content-Type: application/json" https://matic-mumbai.chainstacklabs.com -d '{"id":1,"jsonrpc":"2.0","method":"eth_blockNumber","params":[]}'
    ```

    ![eth_blockNumber](https://s1.ax1x.com/2023/01/16/pSl0HRf.jpg)

- eth_getBalance：返回指定地址的余额。

    ```shell
    curl -s -X POST -H "Content-Type: application/json" https://matic-mumbai.chainstacklabs.com -d '{"id":1,"jsonrpc":"2.0","method":"eth_getBalance","params":["0x3FD761B0106BbE39eD0A627042Bc5B0E95622bdD","latest"]}'
    ```

    ![eth_getBalance](https://s1.ax1x.com/2023/01/16/pSlBFQU.jpg)

- eth_getTransactionCount：返回指定地址的交易数量。

    ```shell
    curl -s -X POST -H "Content-Type: application/json" https://matic-mumbai.chainstacklabs.com -d '{"id":1,"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0x3FD761B0106BbE39eD0A627042Bc5B0E95622bdD","latest"]}'
    ```

    ![eth_getTransactionCount](https://s1.ax1x.com/2023/01/16/pSlBFQU.jpg)

- eth_accounts：返回客户端持有的地址列表。

     ```shell
    curl -s -X POST -H "Content-Type: application/json" https://matic-mumbai.chainstacklabs.com -d '{"id":1,"jsonrpc":"2.0","method":"eth_accounts","params":[]}'
    ```

    ![eth_accounts](https://s1.ax1x.com/2023/01/16/pSlgmuV.jpg)

- eth_getCode：返回指定地址的代码。

    ```shell
    curl -s -X POST -H "Content-Type: application/json" https://matic-mumbai.chainstacklabs.com -d '{"id":1,"jsonrpc":"2.0","method":"eth_getCode","params":["0x3FD761B0106BbE39eD0A627042Bc5B0E95622bdD","0x2"]}'
    ```

    ![eth_getCode](https://s1.ax1x.com/2023/01/16/pSlgTvq.jpg)

## 第 3 题：同一个合约里代码相同的函数，为什么 GAS 费不同？

请用 Remix 验证在同一个合约里，名称不同、代码相同的函数的 GAS 费不相等，并解释原因。

评分标准：

- 验证成功： 10 分，对过程要截图
- 解释正确： 10 分

## 答

- GAS 费用取决于智能合约执行时需要执行的操作（例如存储数据、读取数据、进行计算等）的复杂度。如果同一合约中代码相同的函数所需要执行的操作不同，那么它们的 GAS 费用也可能不同。例如
  - 函数操作一样但是名称不同，那么GAS费会不一样
  - 调用函数时传入的参数不同，如果传入的参数需要消耗更多的 GAS 来执行，那么 GAS 费用就会增加。
  - 智能合约的状态不同，如果智能合约中的数据需要更多的 GAS 来读取或写入，那么 GAS 费用就会增加。
  - 网络状态不同，如果网络繁忙，那么 GAS 价格就会提高，导致 GAS 费用增加。
  - 执行函数时需要调用其他合约，那么 GAS 费用也会受到影响。
- ![验证](https://s1.ax1x.com/2023/01/17/pS1RM2d.jpg)

## 第 4 题：用 Remix 部署校验合约

用 Remix 写一个合约，部署到 mumbai 链上，计算 mumbai 链的最近平均出块时间，并校验合约代码代码。

评分标准：

- 代码正确：截图 10 分
![代码](https://s1.ax1x.com/2023/01/16/pSlX7bF.jpg)
- 部署成功：截图 10 分
![部署成功](https://s1.ax1x.com/2023/01/16/pSlxZB8.jpg)

- 代码校验：截图 5 分
- 获取结果：截图 5 分

## 第 5 题：黑名单 ERC20 合约

### 5.1 修复 BlacklistTokenFactory 合约里的 bug

命令 `yarn deploy:mumbai` 会在 mumbai 链上部署 BlacklistTokenFactory 和一个 BlacklistToken 合约（简称 BT1 ），地址保存在 deploy/deployed/mumbai.json 文件里（提示：删除该文件可以重新部署）。请在区块链浏览器上调用 BlacklistTokenFactory 合约里的 createBlacklistToken 函数，动态创建一个 BlacklistToken 合约（简称 BT2 ）， BT2 的地址保存在 BlacklistTokenFactory 的 blacklistTokens 数组里，也可以在交易里的 CreateBlacklistToken 事件里查看 BT2 的地址。因为目前 BlacklistTokenFactory 合约里有 bug，导致 `yarn test` 报告某些测试用例失败。请在区块链浏览器上比较 BT1 和 BT2 的功能，找出 BT2 功能异常的问题现象。并尝试修复合约 contracts/BlacklistTokenFactory.sol 的代码，能通过文件 test/BlacklistTokenFactory.test.js 里全部的自动化测试。

评分标准：

- 找到问题现象： 截图 10 分
- 解释问题原因： 10 分
- 修改合约代码： 20 分
- 自动化测试全部通过： 10 分

### 5.2 优化 BlacklistTokenFactory.test.js

参考： <https://hardhat.org/tutorial/testing-contracts> , 使用 loadFixture 重构 test/BlacklistTokenFactory.test.js 文件。

评分标准： 30 分

### 5.3 增强 BlacklistToken 合约的功能

修改本代码仓库里的 BlacklistToken 合约代码，在转账的时候抽取 10% 的手续费，并把被扣除的手续费转给合约的创建者。

评分标准：

- 代码正确： 30 分
- 部署成功： 10 分
- 代码校验： 10 分
- 手工测试： 10 分
-

### 5.4 对 BlacklistToken 合约进行自动化测试

目前 BlacklistToken 还没有写测试用例，请在 test/BlacklistToken.test.js 文件里尽量补齐。可参考 <https://hardhat.org/tutorial/testing-contracts> 和网上其它开源项目的测试用例。

评分标准：每个有效的（能执行通过）测试用例 10 分，性质相同的用例不重复计分
