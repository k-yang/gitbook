---
description: Explain the fungible token precompile
---

# Fungible token precompile

The fungible token precompile allows conversions between native coins and ERC20 tokens. Read up on [evm-assets-vs-native-coins.md](../evm-assets-vs-native-coins.md "mention")to understand what a native coin is.



If a native coin doesn't have an ERC20 representation, then a "fungible token mapping" must be created for it. That can be done via the Cosmos msg `MsgCreateFunToken`.

```bash
nibid tx evm create-funtoken \
--bank-denom unibi \
--from validator \
--gas auto \
--gas-adjustment 1.5 \
-y | tx
```

You can verify if a fungible token mapping exists for a native coin by querying for fungible token mappings for a specific denom.

```bash
nibid q evm funtoken unibi | jq
```

## Convert from native coin to ERC20

There are two mechanisms for converting from a native coin to ERC20:

1. Cosmos msg `MsgConvertCoinToEvm`
2. `IFunToken.sendToEvm` precompile call



The `MsgConvertCoinToEvm` msg can be called via command line:

```bash
nibid tx evm convert-coin-to-evm \
0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8 \
1000000unibi \
--from validator \
--gas auto \
--gas-adjustment 1.5 \
-y | tx
```

The `IFunToken.sendToEvm` precompile call can be called via another EVM smart contract or directly via ethers.js:

```typescript
import { HDNodeWallet, JsonRpcProvider } from "ethers";
import { ERC20Minter__factory, IFunToken__factory } from "../../typechain-types";

// connects to local node
const jsonRpcProvider = new JsonRpcProvider("http://localhost:8545");

// mnemonic for the HD wallet
const mnemonic = "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host"
const owner = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/0").connect(jsonRpcProvider)
console.log("owner address:", owner.address)

const FunTokenGatewayAddress = "0x0000000000000000000000000000000000000800"
const ERC20Address = "0x7D4B7B8CA7E1a24928Bb96D59249c7a5bd1DfBe6"
const bankDenom = "unibi"

async function main() {
  const funtokenPrecompile = IFunToken__factory.connect(FunTokenGatewayAddress, owner)

  console.log("owner unibi balance in wei: ", await funtokenPrecompile.bankBalance(owner.address, bankDenom))
  
  const txResponse = await funtokenPrecompile.sendToEvm(bankDenom, 1, owner.address)
  console.log("txResponse: ", txResponse)

  const receipt = await jsonRpcProvider.waitForTransaction(txResponse.hash)
  console.log("receipt: ", receipt)

  console.log("owner unibi balance in wei: ", await funtokenPrecompile.bankBalance(owner.address, bankDenom))
}

main()
```

## Convert from ERC20 to native coin

Likewise, an ERC20 token can be converted back to a native coin if it has a fungible token mapping. The only mechanism to do this is via the `IFunToken.sendToBank` precompile call.

```typescript
import { HDNodeWallet, JsonRpcProvider, Wallet } from "ethers";
import { IFunToken__factory, MyToken__factory } from "../../typechain-types";

// connects to local node
const jsonRpcProvider = new JsonRpcProvider("http://localhost:8545");

// mnemonic for the HD wallet
const mnemonic = "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host"
const owner = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/0").connect(jsonRpcProvider)
Wallet.fromPhrase(mnemonic, jsonRpcProvider)

console.log("owner address:", owner.address)

const FunTokenGatewayAddress = "0x0000000000000000000000000000000000000800"
const ERC20Address = "0x76e03400dC49dD3Dbede29f5e11b0e7bc215F202"
const bankDenom = "erc20/" + ERC20Address
const recipient = "nibi1ltez0kkshywzm675rkh8rj2eaf8et78cqjqrhc"

async function main() {
  const funtokenPrecompile = IFunToken__factory.connect(FunTokenGatewayAddress, owner)

  console.log("recipient bankBalance: ", await funtokenPrecompile.bankBalance("0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8", bankDenom))
  
  const txResponse = await funtokenPrecompile.sendToBank(ERC20Address, 1, recipient)
  console.log("txResponse: ", txResponse)

  const receipt = await jsonRpcProvider.waitForTransaction(txResponse.hash)
  console.log("receipt: ", receipt)

  console.log("recipient bankBalance: ", await funtokenPrecompile.bankBalance("0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8", bankDenom))
}

main()
```
