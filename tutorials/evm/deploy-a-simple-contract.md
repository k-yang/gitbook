---
description: Deploy a Solidity smart contract with Hardhat
---

# Deploy a simple contract

## Initialize Hardhat

Creates a directory that will act as your Hardhat project root.&#x20;

```bash
mkdir hardhat-example
cd hardhat-example

npm init --yes
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox
npx hardhat init
```

Select `Create a TypeScript project` when prompted.

## Compile your contract

You should have a sample `Lock.sol` contract under `contracts/`. Compile it by running

```bash
npx hardhat compile
```

It will generate the compilation output under `artifacts/`. There's no need to look at these contents unless you're interested in low level EVM internals.

## Deploy your contract

Start a [localnet](../run-a-localnet.md). Then update your `hardhat.config.ts` file to the following:

```typescript
// hardhat.config.ts

import "@nomicfoundation/hardhat-toolbox";
import { HardhatUserConfig } from "hardhat/config";

const config: HardhatUserConfig = {
  solidity: "0.8.24",
  networks: {
    nibiru: {
      url: "http://localhost:8545",
      accounts: {
        mnemonic: "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host",
        path: "m/44'/118'/0'/0",
        initialIndex: 0,
        count: 20,
        passphrase: "",
      },
    }
  }
};

export default config;
```

Run the following command to deploy your contract

```bash
npx hardhat --network nibiru \
ignition deploy ./ignition/modules/Lock.ts
```

Hardhat Ignition is a new system for deploying smart contracts. You can read more about it at [https://hardhat.org/ignition/docs/getting-started](https://hardhat.org/ignition/docs/getting-started#overview).

