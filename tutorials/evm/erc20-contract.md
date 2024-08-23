---
description: >-
  You can follow along with the following repo:
  https://github.com/k-yang/evm-contracts
---

# ERC20 Contract

## Faucet

You can skip to [#initialize-hardhat](erc20-contract.md#initialize-hardhat "mention") if you've already obtained funds.

### Generating your wallet address

If you know your address already, you can skip to [#obtain-funds-from-faucet](erc20-contract.md#obtain-funds-from-faucet "mention").

Create a file named `scripts/wallet_address.ts`

```typescript
import { HDNodeWallet } from "ethers";

// mnemonic for the HD wallet
const mnemonic = "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host"
const owner = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/0")

async function main() {
  console.log("owner address:", owner.address)
}

main()
```

Then run

<pre class="language-bash"><code class="lang-bash"><strong>$ npx ts-node scripts/wallet_address.ts
</strong>
owner address: 0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8
</code></pre>

### Obtain funds from faucet

Head to [https://app.nibiru.fi/faucet](https://app.nibiru.fi/faucet), select the correct network from the dropdown in the top right, and enter your wallet address.

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-21 at 10.46.36 AM.png" alt=""><figcaption><p>Nibiru Faucet</p></figcaption></figure>

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

## Deploy and interact with the contract

Next, we will deploy the `MyToken.sol` contract. You can take a look at `scripts/deploy_erc20.ts` to see what the script executes.

<details>

<summary><code>deploy_erc20.ts</code></summary>

```typescript
import { HDNodeWallet, JsonRpcProvider } from "ethers";
import { MyToken__factory } from "../typechain-types";

// connects to local node
const jsonRpcProvider = new JsonRpcProvider("https://evm-rpc.testnet-1.nibiru.fi:443");

// mnemonic for the HD wallet
const mnemonic = "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host"
const owner = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/0").connect(jsonRpcProvider)
const recipient = HDNodeWallet.fromPhrase(mnemonic, "", "m/44'/118'/0'/0/1").connect(jsonRpcProvider)

async function main() {
  console.log("owner address:", owner.address)
  const factory = new MyToken__factory(owner);
  const contract = await factory.deploy(owner, {
    gasPrice: "1",
  });
  console.log("contract address: ", await contract.getAddress())
  await contract.waitForDeployment()

  // obtain the total supply of the ERC-20 contract
  console.log("totalSupply: ", await contract.totalSupply())
  console.log("balanceOf(owner): ", await contract.balanceOf(owner.address))

  // transfer 100 tokens to another address
  const txResponse = await contract.transfer(recipient.address, 100)
  await jsonRpcProvider.waitForTransaction(txResponse.hash)

  console.log("balanceOf(recipient): ", await contract.balanceOf(recipient.address))
  console.log("balanceOf(owner): ", await contract.balanceOf(owner.address))
}

main()
```



</details>

Run the script:

<pre class="language-bash"><code class="lang-bash"><strong>$ npx ts-node scripts/deploy_erc20.ts
</strong>
owner address: 0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8
contract address:  0x26F3eF133b3045D5D362d8eB3892b98674dF4590
totalSupply:  1000000000000000000000000n
balanceOf(owner):  1000000000000000000000000n

balanceOf(recipient):  100n
balanceOf(owner):  999999999999999999999900n
</code></pre>
