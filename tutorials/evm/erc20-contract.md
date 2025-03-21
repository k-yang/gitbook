---
description: How to deploy an ERC20 contract on a localnet.
---

# ERC20 Contract

## Prerequisites

Run a localnet: [run-locally.md](../run-locally.md "mention")

## Initialize project directory

Clone the sample repo and compile the contracts.

```bash
git clone https://github.com/k-yang/evm-contracts.git
cd evm-contracts

npm install

# compile contract
npx hardhat compile
```

It will generate the compilation output under `artifacts/`. There's no need to look at these contents unless you're interested in low level EVM internals.&#x20;

## Deploy the contract

Next, we will deploy the `MyToken.sol` contract. You can take a look at `scripts/ERC20/deploy.ts` to see what the script executes.

<details>

<summary>deploy.ts</summary>

```typescript
import { HDNodeWallet, JsonRpcProvider } from "ethers";
import { MyToken__factory } from "../../typechain-types";

// connects to local node
const jsonRpcProvider = new JsonRpcProvider("http://localhost:8545");

// mnemonic for the HD wallet
const mnemonic = "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host"
const owner = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/0").connect(jsonRpcProvider)
const recipient = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/1").connect(jsonRpcProvider)

async function main() {
  console.log("owner address:", owner.address)
  const factory = new MyToken__factory(owner);
  const erc20 = await factory.deploy(owner, {
    gasPrice: "1",
  });
  const erc20Addr = await erc20.getAddress()
  console.log("ERC20 contract address: ", erc20Addr)
  await erc20.waitForDeployment()

  // obtain the total supply of the ERC-20 contract
  console.log("totalSupply: ", await erc20.totalSupply())
  console.log("balanceOf(owner): ", await erc20.balanceOf(owner.address))
}

main()
```

</details>

Run the script:

<pre class="language-bash"><code class="lang-bash"><strong>$ npx ts-node scripts/ERC20/deploy.ts
</strong>
owner address: 0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8
contract address:  0x26F3eF133b3045D5D362d8eB3892b98674dF4590
totalSupply:  1000000000000000000000000n
balanceOf(owner):  1000000000000000000000000n
</code></pre>
