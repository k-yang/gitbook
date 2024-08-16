# Send ERC-20 tokens to bank

First, start a [localnet](../tutorials/run-a-localnet.md)&#x20;

## Create a fungible token mapping from an ERC-20 contract

First, deploy an [ERC-20 contract](../tutorials/evm/deploy-an-erc20-contract.md). Then, run the following command:

```bash
CONTRACT_ADDRESS=0x76e03400dC49dD3Dbede29f5e11b0e7bc215F202

nibid tx evm create-funtoken-from-erc20 \
$CONTRACT_ADDRESS \
--from validator \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

There should be a `EventFunTokenCreated` event that displays the new bank coin denom.

<details>

<summary>Output</summary>

```json
{
  "type": "eth.evm.v1.EventFunTokenCreated",
  "attributes": [
    {
      "key": "bank_denom",
      "value": "\"erc20/0x76e03400dC49dD3Dbede29f5e11b0e7bc215F202\"",
      "index": true
    },
    {
      "key": "creator",
      "value": "\"nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl\"",
      "index": true
    },
    {
      "key": "erc20_contract_address",
      "value": "\"0x76e03400dC49dD3Dbede29f5e11b0e7bc215F202\"",
      "index": true
    },
    {
      "key": "is_made_from_coin",
      "value": "false",
      "index": true
    }
  ]
}
```



</details>

## Send ERC-20 tokens to bank

Requires calling the FunToken precompile, which can be done with `ethers.js`.&#x20;

```typescript
import { HDNodeWallet, JsonRpcProvider, Wallet } from "ethers";
import { IFunToken__factory } from "../typechain-types";

// connects to local node
const jsonRpcProvider = new JsonRpcProvider("http://localhost:8545");

// mnemonic for the HD wallet
const mnemonic = "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host"
const owner = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/0").connect(jsonRpcProvider)
Wallet.fromPhrase(mnemonic, jsonRpcProvider)

const FunTokenGatewayAddress = "0x0000000000000000000000000000000000000800"
const ERC20Address = "0x76e03400dC49dD3Dbede29f5e11b0e7bc215F202"
const to = "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"

async function main() {
  console.log("owner address:", owner.address)
  const contract = IFunToken__factory.connect(FunTokenGatewayAddress, owner)

  const txResponse = await contract.bankSend(ERC20Address, 1, to)
  console.log(txResponse)
  console.log(await jsonRpcProvider.waitForTransaction(txResponse.hash))
}

main()
```
