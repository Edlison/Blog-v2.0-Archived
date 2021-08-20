---
title: ETH - 发行自己的Token
catagories:
- BlockChain
tags:
- eth
---

# 开始

通过以太坊的**智能合约**可以编写自己的代币(Token)，可以代表任何可以交易的东西，如：积分，财产，证书等等。

# 合约代码

如果想要在以太坊的区块链上创建代币，就需要实现它的**代币标准**。

## ZZC Token源码

```solidity
pragma solidity ^0.4.16;

interface tokenRecipient { function receiveApproval(address _from, uint256 _value, address _token, bytes _extraData) public; }

contract TokenERC20 {
    string public name;
    string public symbol;
    uint8 public decimals = 18;  // 18 是建议的默认值
    uint256 public totalSupply;

    mapping (address => uint256) public balanceOf;  //
    mapping (address => mapping (address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Burn(address indexed from, uint256 value);


    function TokenERC20(uint256 initialSupply, string tokenName, string tokenSymbol) public {
        totalSupply = initialSupply * 10 ** uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        name = tokenName;
        symbol = tokenSymbol;
    }


    function _transfer(address _from, address _to, uint _value) internal {
        require(_to != 0x0);
        require(balanceOf[_from] >= _value);
        require(balanceOf[_to] + _value > balanceOf[_to]);
        uint previousBalances = balanceOf[_from] + balanceOf[_to];
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        Transfer(_from, _to, _value);
        assert(balanceOf[_from] + balanceOf[_to] == previousBalances);
    }

    function transfer(address _to, uint256 _value) public returns (bool) {
        _transfer(msg.sender, _to, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= allowance[_from][msg.sender]);     // Check allowance
        allowance[_from][msg.sender] -= _value;
        _transfer(_from, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public
        returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        return true;
    }

    function approveAndCall(address _spender, uint256 _value, bytes _extraData) public returns (bool success) {
        tokenRecipient spender = tokenRecipient(_spender);
        if (approve(_spender, _value)) {
            spender.receiveApproval(msg.sender, _value, this, _extraData);
            return true;
        }
    }

    function burn(uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value);
        balanceOf[msg.sender] -= _value;
        totalSupply -= _value;
        Burn(msg.sender, _value);
        return true;
    }

    function burnFrom(address _from, uint256 _value) public returns (bool success) {
        require(balanceOf[_from] >= _value);
        require(_value <= allowance[_from][msg.sender]);
        balanceOf[_from] -= _value;
        allowance[_from][msg.sender] -= _value;
        totalSupply -= _value;
        Burn(_from, _value);
        return true;
    }
}

```

# 部署合约

## 方式

1. Geth编译代码
2. etherrum wallet钱包部署合约
3. MetaMask钱包 + Remix Solidity IDE部署

## Meta Mask + Remix Solidity IDE

1. 创建钱包，转到测试网络Ropsten.
2. 在Remix Solidity IDE中对合约代码进行编程。
3. 对其**编译**

选择对应的编译器版本。

4. 进行**发布**

Environment: Injected Web3

Account: xxxxx(会自动读取Meta Mask的账户)

选择编译好的文件

构造函数传参

在Meta Mask中对合约部署进行确认。

5. 添加自定义的Token

# 公布源码

在etherscan.io上进入到合约地址。

贴上源代码，它会对**源码进行编译**，将字节码与区块`DATA`内的**字节码进行比对**，需要完全一样。

之后就可以ICO了。







----

Reference

https://learnblockchain.cn/2018/01/12/create_token

https://blog.csdn.net/java_hhhh/article/details/79771752