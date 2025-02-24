# FAQ

## What is the first block of Nibiru’s EVM?

[18538950](https://nibiscan.io/block/18538950)

## How does Nibiru’s wei differ from Ethereum’s wei?

Because Nibiru started out as a Cosmos blockchain, the native gas token NIBI has a decimal denomination of 6. That means the smallest transferrable unit is one micro-NIBI (1 unibi, or 10^-6 NIBI).

However, on Ethereum, the smallest transferrable unit is one wei == 10^-18 ETH. To maintain compatibility with smart contracts that depend on the 18 decimals of precision, we decided to make Nibiru’s EVM recognize 10^18 wei as 1 NIBI. But since Nibiru’s smallest transferrable unit is actually 10^-6 NIBI (due to the Cosmos SDK limitation), the smallest unit transferrable on Nibiru is 10^12 wei.

For developers, that means that precision digits smaller than 10^12 wei will be truncated. For example, 1,999,999,999,999 wei will be truncated as 1,000,000,000,000 wei. If you’re doing gas estimations, it would be prudent to add 10^12 wei to the estimated gas amount to ensure that you’re not affected by this truncation issue.

It also means that transfers are truncated to 10^12 wei. In practice, that means amounts smaller than 1 micro-NIBI are truncated, but that should have minimal monetary impact, as we’re talking about tiny fractions of a cent.

## What is the source of RANDDAO for blocks?

The randomness comes from [the block time and the last commit hash](https://github.com/NibiruChain/nibiru/blob/0caad783994d5ed7ab6db00afa029667abcc70d8/x/evm/keeper/msg_server.go#L123-L125).&#x20;

## Is NIBI also an ERC-20 token?

NIBI itself is not an ERC-20, but there is a WNIBI ERC-20 contract, similar to how ETH has WETH on Ethereum.

The contract address is [0x7D4B7B8CA7E1a24928Bb96D59249c7a5bd1DfBe6](https://nibiscan.io/address/0x7D4B7B8CA7E1a24928Bb96D59249c7a5bd1DfBe6)

## If I get USDC in WASM via IBC and added some more in EVM side via a bridge will I see 2 different token balances or just one?

Two different token balances

## Do EVM and wasm txs occur in parallel?

No, serially

## Which consumes more gas? EVM or wasm?

It depends on the smart contract
