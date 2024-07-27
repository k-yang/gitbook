# Deploy an ERC20 contract

## Create the contract

Create a `MyToken.sol` file under `contracts/` and enter the following code

```solidity
// SPDX-License-Identifier: MIT
// Compatible with OpenZeppelin Contracts ^5.0.0
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";

contract MyToken is ERC20, ERC20Burnable, Ownable, ERC20Permit {
    constructor(address initialOwner)
        ERC20("MyToken", "MTK")
        Ownable(initialOwner)
        ERC20Permit("MyToken")
    {
        _mint(msg.sender, 1000000 * 10 ** decimals());
    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }
}
```

Then create a file named `MyToken.ts` under `ignition/modules` and write the following code

```typescript
import { buildModule } from "@nomicfoundation/hardhat-ignition/modules";

const MyTokenModule = buildModule("MyTokenModule", (m) => {
  const owner = m.getAccount(0);
  const myToken = m.contract("MyToken", [owner]);

  return { myToken: myToken };
});

export default MyTokenModule;
```

Next run

```
npx hardhat compile
npx hardhat --network nibiru ignition deploy ./ignition/modules/MyToken.ts
```

You should end up with the contract address in the console output, like so

```
Deployed Addresses

MyTokenModule#MyToken - 0xe5F54D19AA5c3c16ba70bC1E5112Fe37F1764134
```

## Interact with the contract

Next, create a file named `main.ts` in the root folder.

```typescript
import { HDNodeWallet, JsonRpcProvider } from "ethers";
import { MyToken__factory } from "./typechain-types";

async function main() {
  const provider = new JsonRpcProvider("http://localhost:8545");

  const owner = HDNodeWallet.fromPhrase("guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host", "", "m/44'/118'/0'/0/0").connect(provider)
  const contract = MyToken__factory.connect("0x76e03400dC49dD3Dbede29f5e11b0e7bc215F202", owner);
  console.log(await contract.totalSupply())
  
  const alice = HDNodeWallet.fromPhrase("guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host", "", "m/44'/118'/0'/0/1").connect(provider)

  const transferTx = await contract.transfer(alice.address, BigInt(1000))
  await transferTx.wait();
  console.log(await contract.balanceOf(owner.address))
  console.log(await contract.balanceOf(alice.address))
}

main()
```

Then run

```bash
npx ts-node main.ts
```

You should see the owner's balance decrease and alice's balance increase.
