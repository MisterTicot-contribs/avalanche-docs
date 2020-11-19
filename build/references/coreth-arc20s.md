# Coreth ANTs and ARC-20s

## ANTs on the C-Chain

### Why move an ANT from the X-Chain to the C-Chain?

An Avalanche Native Token (ANT) is any token created on the X-Chain. These tokens can be exchanged on the X-Chain at lightning fast speeds taking advantage of the superior performance offered by using a DAG instead of a strictly linear chain.

When we want to use smart contract functionality, we need a total ordering of state transitions (transactions), so that each transaction can modify the state of the network without having to worry about external dependencies.

Therefore, when we want to use smart contract functionality, we must perform an atomic swap to move ANTs from the X-Chain to the C-Chain.

### ANTs on the C-Chain

#### Native Coin - AVAX

ANTs on the C-Chain are either AVAX &emdash; the Avalanche native asset &emdash; or another asset that was created on the X-Chain.

The C-Chain is a modified instance of the Ethereum Virtual Machine (EVM), so AVAX on the C-Chain is equivalent to ETH on the Ethereum network. When you create or call a smart contract, you pay for gas costs with AVAX. You can natively transfer AVAX between accounts, send it to a smart contract, all using native EVM tools and libraries.

An EVM Transaction is composed of the following fields:

* **`nonce`** Scalar value equal to the number of transactions sent by the sender
* **`gasPrice`** Scalar value equal to the number of Wei (1 Wei = 10^-18 AVAX) paid per unit of gas to execute this transaction
* **`gasLimit`** Scalar value equal to the maximum amount of gas that should be used in executing this transaction
* **`to`** The 20 byte address of the message call's recipient. If the transaction is creating a contract, `to` is left empty
* **`value`** Scalar value of native asset (AVAX) measured in Wei (1 Wei = 10^-18 AVAX) to be transferred to the message call's recipient or in the case of a contract creation, as an endowment to the newly created contract.
* **`v, r, s`** Values corresponding to the signature of the transaction.
* **`data`** Unlimited size byte array specifying the input data to a contract call or (if creating a contract) the EVM bytecode for the account initialization process

#### Native Assets - Tokens

Non-AVAX Assets, however, have no native representation within the EVM. Therefore, Coreth makes minor modifications to the EVM and state management in order to support asset balances and the ability to transfer these assets on the C-Chain.

Non-AVAX Assets (ANTs) are supported on the C-Chain by keeping an account balance mapping [assetID -> balance]. These assets can be exported back to the X-Chain or moved around on the C-Chain using `assetCall` and `assetBalance`.

##### assetCall

`assetCall` is a precompiled contract at address `0xassetCallAddr`. `assetCall` allows users to atomically transfer a native asset to a given address and (optionally) also make a contract call to that same address. This is parallel to how a normal transaction can send value to some address, and atomically call that address with some `data`.


```text
assetCall(address addr, uint256 assetID, uint256 assetAmount, bytes memory callData) -> {ret: bytes memory}
```

These arguments can be packed by `abi.encodePacked(...)` in Solidity since there is only one argument with variadic length (callData). The first three arguments are constant length, so the precompiled contract simply parses the call `data` input as:


```text
+-------------+---------------+--------------------------------+
| address     : address       |                       20 bytes |
+-------------+---------------+--------------------------------+
| assetID     : uint256       |                       32 bytes |
+-------------+---------------+--------------------------------+
| assetAmount : uint256       |                       32 bytes |
+-------------+---------------+--------------------------------+
| callData    : bytes memory  |            len(callData) bytes |
+-------------+---------------+--------------------------------+
                              |       84 + len(callData) bytes |
                              +--------------------------------+
```

##### assetBalance

`assetBalance` is a precompiled contract at address `0xassetBalance`. `assetBalance` is the ANT equivalent of using `balance` to get the AVAX balance.

```text
assetBalance(address addr, uint256 assetID) -> {balance: uint256}
```

These arguments can be packed by `abi.encodePacked(...)` in Solidity since all of the arguments have constant length.

```text
+-------------+---------------+-----------------+
| address     : address       |        20 bytes |
+-------------+---------------+-----------------+
| assetID     : uint256       |        32 bytes |
+-------------+---------------+-----------------+
                              |        52 bytes |
                              +-----------------+
```

## ARC-20s on the C-Chain

An ARC-20 is the Avalanche equivalent of an ERC-20.

### What is an ERC-20

An ERC-20 (Ethereum Request for Comment 20) is a standardized token on Ethereum. It presents a standard set of functions and events that allow a smart contract to serve as a token on Ethereum. For the complete explanation, read the original proposal heree: https://eips.ethereum.org/EIPS/eip-20.

ERC-20s expose the following interface:

```boo
// Functions
function name() public view returns (string)
function symbol() public view returns (string)
function decimals() public view returns (uint8)
function totalSupply() public view returns (uint256)
function balanceOf(address _owner) public view returns (uint256 balance)
function transfer(address _to, uint256 _value) public returns (bool success)
function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
function approve(address _spender, uint256 _value) public returns (bool success)
function allowance(address _owner, address _spender) public view returns (uint256 remaining)

// Events
event Transfer(address indexed _from, address indexed _to, uint256 _value)
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```

As a smart contract, ERC-20s maintain their own state. This means that if your account owns 5 DogeCoin (a popular ERC-20), then this is stored as an account balance in the DogeCoin contract, instead of on your account's own storage. Therefore, ERC-20s are second class citizens in the world of Ethereum, but expose a powerful interface enabling a rich world of DeFi.

### From ANT to ARC-20

Avalanceh Native Tokens (ANTs) are stored directly on the account that owns them. In order to make ANTs easier to use in smart contracts on the C-Chain, we would like to give them the same interface as an ERC-20. We'll call this wrapped asset an ARC-20. To do this, we need to create an ERC-20 contract with one additional field: `assetID`. The `assetID` is a unique hash that identifies an asset across the Avalanche Platform.

Now, we will deploy an ERC-20 smart contract with two additional functions to support deposits and withdrawals of the underlying ANT into the contract. To implement this, we will use `assetCall` and `assetBalance` precompiled contracts.

#### ARC-20 Deposits

Similar to WETH, in order to deposit some funds into an ARC-20, we need to send the ARC-20 contract the deposit amount and then invoke the contract's deposit function, so that it can acknowledge the deposit and update its balance for the caller's account. With WETH, this can be accomplished with a simple `call` because this combines sending the native coin to an address and (optionally) calling that address with some `callData`. With non-AVAX ARC-20s, this can be accomplished through the following:

* **`nonce`**: 2
* **`gasPrice`**: 470 gwei
* **`gasLimit`**: 3000000
* **`to`**: `0xassetCallAddr`
* **`value`**: 0
* **`v, r, s`**: Transaction Signature
* **`data`**: abi.encodePacked(arc20Address, assetID, assetAmount, abi.encodeWithSignature("deposit()"))

This transfers `assetAmount` of `assetID` to the addresss of the ARC-20 and then invokes the function `deposit()` on the ARC-20. 

The ARC-20 contract must keep an up to date balance of how much of `assetID` it owns. When it receives a `deposit()` call, it will check its current balance of `assetID` and calculate the deposit amount by subtracting the update balance from what its balance was prior to the call.

#### ARC-20 Withdrawals

Like ERC-20s, ARC-20s maintain an account balance for every account that owns some funds. When an ARC-20 receives a withdraw request, it can simply check that the caller account has a sufficient balance, then use `assetCall` to send the caller the requested amount of the underlying ANT.
