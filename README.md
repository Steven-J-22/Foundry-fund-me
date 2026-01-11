# Foundry FundMe

一个基于 **Solidity** 开发、使用 **Foundry** 框架进行测试和部署的去中心化众筹智能合约。该项目实现了基本的ETH募资功能，并集成了 **Chainlink 预言机** 来确保募资符合最低美元金额要求，同时实践了安全的合约所有权管理和资金提取模式。

## 🚀 项目概述

`FundMe` 合约允许用户以ETH进行资助，并利用Chainlink预言机获取实时ETH/USD价格，以强制执行最低资助门槛（例如5美元）。只有合约所有者可以一次性提取合约内所有资金，该过程通过安全的权限修饰器和提款函数实现。

本项目作为学习Solidity与Foundry的实践项目，涵盖了智能合约开发、测试、部署的全流程。

## 📁 项目结构

## 🛠 技术栈

*   **Solidity**: 智能合约开发语言
*   **Foundry**: 合约开发、测试与部署框架 (使用 `forge` 和 `cast`)
*   **Chainlink**: 去中心化预言机网络，提供链上价格数据
*   **Solidity Scripting**: 使用Solidity编写部署脚本

## 🔧 核心功能

1.  **资助功能 (`fund`)**: 用户发送ETH调用`fund()`函数，合约通过Chainlink预言机验证发送的ETH价值是否大于预设的 `MINIMUM_USD`。
2.  **价格转换库 (`PriceConverter`)**: 一个独立的`library`，封装了从Chainlink预言机获取ETH/USD价格并进行单位转换的逻辑，实现了代码复用。
3.  **安全的权限控制 (`onlyOwner`)**: 使用自定义的`modifier`来限制关键函数（如`withdraw`）仅允许合约部署者调用。
4.  **安全的资金提取 (`withdraw`)**: 所有者可以提取合约全部余额。提款函数使用低级的`call`方法，并检查执行结果，避免了`transfer`或`send`可能存在的Gas限制问题。
5.  **数据追踪**: 使用`mapping`记录每个地址的资助总额，并使用`array`记录所有资助者地址。

## 📖 快速开始

### 前提条件
1. 安装 [Foundry](https://book.getfoundry.sh/getting-started/installation)。
2. 获取一个Sepolia测试网的RPC URL（例如来自 [Infura](https://infura.io/) 或 [Alchemy](https://www.alchemy.com/)）。
3. 获取一个以太坊钱包的私钥（用于部署，**务必使用测试账户**）。
4. 获取一个 [Etherscan API Key](https://etherscan.io/apis)（用于验证合约）。

### 安装与测试
1.  **克隆仓库**
    ```bash
    git clone https://github.com/Steven-J-22/Foundry-fund-me.git
    cd Foundry-fund-me
    ```

2.  **安装依赖**
    ```bash
    forge install
    ```

3.  **配置环境变量**
    ```bash
    cp .env.example .env
    # 编辑 .env 文件，填入你的 SEPOLIA_RPC_URL, PRIVATE_KEY 和 ETHERSCAN_API_KEY
    ```

4.  **运行测试**
    ```bash
    forge test
    ```
    *或运行更详细的测试：*
    ```bash
    forge test -vvv
    ```

### 部署到Sepolia测试网
1.  **构建项目**
    ```bash
    forge build
    ```

2.  **部署合约**
    ```bash
    source .env
    forge script script/FundMe.s.sol:FundMeScript --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify --etherscan-api-key $ETHERSCAN_API_KEY -vvvv
    ```
    执行此命令将编译、部署合约，并自动在Etherscan上验证源代码。

## 🧪 测试说明
测试文件 `test/FundMe.t.sol` 使用Foundry的测试框架编写，覆盖了以下关键场景：
*   单个用户资助合约。
*   多个用户资助合约。
*   资助金额低于最低要求时的失败情况。
*   非合约所有者尝试提取资金时的失败情况。
*   合约所有者成功提取全部资金。

运行 `forge test` 来确保所有功能按预期工作。

## 🔗 相关链接
*   **Foundry Book**: [https://book.getfoundry.sh/](https://book.getfoundry.sh/)
*   **Solidity Documentation**: [https://docs.soliditylang.org/](https://docs.soliditylang.org/)
*   **Chainlink Data Feeds**: [https://docs.chain.link/data-feeds](https://docs.chain.link/data-feeds)

## 📄 许可证
本项目基于 [MIT License](LICENSE) 授权。
