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

#### EVM Transaction

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

An ARC-20 is the Avalanche equivalent of an ERC-20. An ERC-20 is just a smart contract that maintains balances for accounts and provides an incredibly useful, standardized interface for smart contract developers.

ERC-20s can be deployed directly to the C-Chain without modification, but these contracts do not have a native asset equivalent. Therefore, we create `Wrapped ARC-20s` (`WARC-20s`) which wrap native assets into an ERC-20 interface, so that `ANTs` can be used following the same interface as an ERC-20.

To do this, we can use any ERC-20 contract with added `deposit(uint256 amount)` and `withdraw(uint256 amount)` added to their interface. Withdrawals are easy to implement, as the `WARC-20` can simply verify that the withdrawer has a sufficient account balance and can then send the asset to the withdrawer.

For deposits, we need to invoke `assetCall` in order to atomically send a native asset to a contract and then invoke the deposit function, so that the smart contract can acknowledge that it has received the funds and update its balance for the sender.

This can be done within a smart contract or within a single transaction as follows:

* **`nonce`**: 2
* **`gasPrice`**: 470 gwei
* **`gasLimit`**: 3000000
* **`to`**: `0xassetCallAddr`
* **`value`**: 0
* **`v, r, s`**: Transaction Signature
* **`data`**: abi.encodePacked(warc20Address, assetID, assetAmount, abi.encodeWithSignature("deposit(uint256)", assetAmount))
