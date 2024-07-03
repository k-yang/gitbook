---
description: Deploy a simple Solidity smart contract with Hardhat
---

# Deploy a contract

## Initialize a Hardhat project

Creates a directory that will act as your Hardhat project root.&#x20;

```bash
mkdir hardhat-tutorial
cd hardhat-tutorial

npm init --yes
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox
npx hardhat init
```

Select `Create a TypeScript project` when prompted.

## Compile your contract

Running `npx hardhat init` should give you a sample `Lock.sol` contract under `contracts/`. Let's compile it by running the following command in your Hardhat project root.

```bash
npx hardhat compile
```

It will put the compilation output under `artifacts/`. There's no need to look at these contents unless you're really interested in EVM internals.

## Deploy your contract

Hardhat comes pre-configured with a local node to test contracts against. Run the following code to deploy your `Lock.sol` contract.

```
npx hardhat ignition deploy ./ignition/modules/Lock.ts
```

Hardhat Ignition is a new declarative system for deploying smart contracts. You can read more about it at [https://hardhat.org/ignition/docs/getting-started](https://hardhat.org/ignition/docs/getting-started#overview).

