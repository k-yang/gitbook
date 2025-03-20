---
description: Explain the differences between native x/bank coins and Nibiru EVM assets
---

# EVM assets vs native coins

## What is a native coin?

A "native coin" refers to a coin that exists in Nibiru's x/bank module.

What is the x/bank module? Nibiru Chain was built using the Cosmos SDK. Every Cosmos SDK chain comes pre-wired with an x/bank module which tracks user balances for multiple denoms, and has metadata for those denoms.

For those experienced in EVM, the x/bank module could be considered an ERC1155 implementation written in Golang.

You use the x/bank module whenever you do something like:

```bash
nibid tx bank send \
<from> \
<to> \
1000000unibi \
--fees 5000unibi \
--sign-mode amino-json \
--yes | tx
```

## Gas Token

`unibi` is a native coin that is also acts as the gas token for the chain. All transaction gas fees are paid in `unibi`. It also acts as the gas token for Nibiru's EVM, which is similar to `ether` in Ethereum. Therefore, an account must have `unibi` to send EVM transactions.&#x20;

## ERC20 Tokens

ERC20 tokens, on the other hand, exist as independent smart contracts in EVM land. They track their own balances internally, and that tracking is separate from the x/bank module. At its core, ERC20 balances are just storage slots in the EVM state tree.

So, how do we bridge the gap between x/bank coins and ERC20 tokens? At Nibiru Chain, we created a fungible token mapping to easily convert between x/bank coins and ERC20 tokens.

<figure><img src="../.gitbook/assets/bank coins (1).jpg" alt=""><figcaption></figcaption></figure>

Let’s take `unibi` as an example. `unibi` is the native gas token on Nibiru Chain. It's similar to `ether` on Ethereum. In its native state, `unibi` and `ether` are not ERC-20 compatible. However, on Ethereum, somebody made an ERC20 representation of ether, called WETH or “wrapped ether”. Similarly, on Nibiru Chain, there is an ERC20 representation of NIBI called WNIBI. Once wrapped as WNIBI, it can be used just like any other ERC20 asset in Nibiru’s EVM.
