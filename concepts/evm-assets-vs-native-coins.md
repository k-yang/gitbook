---
description: Explain the differences between native x/bank coins and Nibiru EVM assets
---

# EVM assets vs native coins

Due to Nibiru’s dual nature as a layer 1 built using the Cosmos SDK with an EVM execution client, Nibiru Chain supports both Cosmos SDK x/bank coins and EVM assets. For the purposes of this discussion, EVM assets are also referred to as ERC20 tokens.

Every Cosmos SDK chain comes pre-wired with an x/bank module. The x/bank module is conceptually an ERC1155 implementation written in Golang. It tracks user balances for multiple token denoms.

ERC20 tokens, on the other hand, exist as independent smart contracts in EVM land. They track their own balances internally, and that tracking is separate from the x/bank module. At its core, ERC20 balances are just storage slots in the EVM state tree.

So, how do we bridge the gap between x/bank coins and ERC20 tokens? At Nibiru Chain, we created a fungible token mapping to easily convert between x/bank coins and ERC20 tokens.

<figure><img src="../.gitbook/assets/bank coins (1).jpg" alt=""><figcaption></figcaption></figure>

Let’s take NIBI as an example. NIBI is the native gas token on Nibiru Chain and exists as an x/bank coin. It’s similar to ether on Ethereum. In the native state, NIBI and ether are not ERC-20 compatible. However, on Ethereum, somebody made an ERC20 representation of ether, called WETH or “wrapped ether”. Similarly, on Nibiru Chain, there is an ERC20 representation of NIBI called WNIBI.

The similarities end here. To create WETH from ether, one must call the correct smart contract functions to deposit ether and receive WETH. On Nibiru Chain, it’s different. One has to send a specific Cosmos tx to convert from NIBI to WNIBI, because NIBI existed as a Cosmos asset prior to EVM launching.

We also [built a UI](https://app.nibiru.fi/portfolio) to do this.&#x20;

Once wrapped as WNIBI, it can be used just like any other ERC20 asset in Nibiru’s EVM.
