# EVM vs Cosmos Addresses

## Background

### Mnemonics

A mnemonic, also known as a seed phrase, is a sequence of 12 or 24 random English words that derives a private key. An example is `guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host`.

A Hierarchical Deterministic (HD) path allows multiple private keys to be generated from a single seed phrase. HD paths are of the form `m/purpose'/coin_type'/account'/change/index`.

Ethereum uses an HD path of `m/44'/60'/0'/0/0`.

Cosmos uses an HD path of `m/44'/118'/0'/0/0`.

That means the same seed phrase will derive different private keys on Bitcoin, Ethereum, and Nibiru Chain.

### Private Key -> Public Key

Both Cosmos and Ethereum use elliptic curve cryptography, and specifically they both use the `secp256k1` elliptic curve. A private key is simply a point on the curve. Because of the mathematical relationship of the curve, a public key can be derived from the private key, but not the other way around.

<figure><img src="../.gitbook/assets/Screenshot 2025-02-24 at 2.57.17 PM.png" alt=""><figcaption><p>Fig 1: secp256k1 curve</p></figcaption></figure>

For example, the private key `FF15BD81CDDF469FD807C069F0542E5287EED9009888B71D7CD6925A456487D0` derives the public key `02FCF004EAE263CB15C045EB5DFD605DA9E14FD8A695F59E5162D0F29331AD1B20.`

### Public Key -> Address Derivation

An address is not exactly equal to the public key, but is a hash of the public key. Different blockchains use different hashing functions.

Ethereum uses the last 20 bytes of the Keccak-256 hash of the public key as the address. Cosmos uses `RIPEMD160(SHA256(pubKey))` as the address. That means that given the same public key, the EVM and Cosmos addresses will be different.

For a public key of `02FCF004EAE263CB15C045EB5DFD605DA9E14FD8A695F59E5162D0F29331AD1B20`, the Ethereum address bytes is `FAF227DAD0B91C2DEBD41DAE71C959EA4F95F8F8` and the Cosmos address bytes is `177AC608D913F3FE4DAB9C5409D8BFB6CF6DE202`.

### Address Encoding

Furthermore, the string representation of the address bytes differs between blockchains. Ethereum is simple; it simply uses the hex representation of the address bytes as the string. Cosmos uses Bech32 to encode the address bytes into a string.

For example, the address bytes `FAF227DAD0B91C2DEBD41DAE71C959EA4F95F8F8` is represented as `0xFAF227DAD0B91C2DEBD41DAE71C959EA4F95F8F8`, which is what you see in wallets like MetaMask. The same address bytes encoded as Bech32 is `nibi1ltez0kkshywzm675rkh8rj2eaf8et78cqjqrhc`.

## Summary

The table below shows all possible outcomes for the seed phrase `guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host`.

| HD Path                                  | `m/44'/60'/0'/0/0`                                                                                                                   | `m/44'/118'/0'/0/0`                                                                                                                  |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| Private Key                              | `68E80819679ABCCDDFA31064EA84B2FE6870B1EAA0EBE2A1FF40A38533CFAB8B`                                                                   | `FF15BD81CDDF469FD807C069F0542E5287EED9009888B71D7CD6925A456487D0`                                                                   |
| Public Key (Compressed)                  | `026E4A3BEE84DF12A20CB435AAA529B2E2E81F35731E679693CE1AA459CE0980F5`                                                                 | `02FCF004EAE263CB15C045EB5DFD605DA9E14FD8A695F59E5162D0F29331AD1B20`                                                                 |
| Public Key (Uncompressed)                | `046E4A3BEE84DF12A20CB435AAA529B2E2E81F35731E679693CE1AA459CE0980F53566914E3003EBD626BC1768506FC1CF45A4A9BF5049E28F4EEE508875AEF490` | `04FCF004EAE263CB15C045EB5DFD605DA9E14FD8A695F59E5162D0F29331AD1B201B39BD254661402973A6D68099AB8910E17F7730EB3FA5E114E0153BB9131D92` |
| Ethereum Address Bytes (Keccak-256)      | `0xC0F4B45712670CF7865A14816BE9AF9091EDDA1D`                                                                                         | `0xFAF227DAD0B91C2DEBD41DAE71C959EA4F95F8F8`                                                                                         |
| Ethereum Address Bytes as Bech32         | `nibi1cr6tg4cjvux00pj6zjqkh6d0jzg7mksaywxyl3`                                                                                        | `nibi1ltez0kkshywzm675rkh8rj2eaf8et78cqjqrhc`                                                                                        |
| Cosmos Address Bytes RIPEMD160(SHA256()) | `0xF317E8AC48EFDE20EA094B9F48EF1ECCEA683D5C`                                                                                         | `0x177AC608D913F3FE4DAB9C5409D8BFB6CF6DE202`                                                                                         |
| Cosmos Address Bytes as Bech32           | `nibi17vt73tzgal0zp6sffw053mc7en4xs02u4mnvn5`                                                                                        | `nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl`                                                                                        |

Given one mnemonic, we can end up with four different addresses (eight, if you count the bech32 encodings as distinct) depending on which HD Path we choose and if we use the Ethereum hashing algorithm or the Cosmos hashing algorithm for address derivation.
