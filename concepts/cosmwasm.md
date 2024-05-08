# CosmWasm

## Why do we need to store and instantiate contracts?

Ethereum contracts are deployed from a single call. CosmWasm requires two calls to create a contract: `store` and `instantiate`. The reason is that it's more space efficient.



Image you have two identical contracts. In Ethereum, the two contracts would take up double the space, even though the underlying bytecode is the exact same. In CosmWasm, the contracts point reference a code id which is basically the bytecode, so contract instances are essentially pointers to the same underlying bytecode. There's no data duplication for two identical contracts. As soon as contracts differ though, new bytecode has to be stored.&#x20;
