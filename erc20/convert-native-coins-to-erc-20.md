---
description: Showcases the fungible token mapping capabilities
---

# Convert native coins to ERC-20

First, start a [localnet](../tutorials/run-locally.md)&#x20;

## \[One time] Create a fungible token mapping from a bank coin

Only required if the fungible token mapping doesn't exist. For \`unibi\`, it already exists at \``` 0x7D4B7B8CA7E1a24928Bb96D59249c7a5bd1DfBe6` ``. To create a fungible token mapping, run the following:

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx evm create-funtoken unibi \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

There should be a `EventFunTokenCreated` event that displays the newly deployed ERC-20 contract.

<details>

<summary>Output</summary>

```json
{
  "type": "eth.evm.v1.EventFunTokenCreated",
  "attributes": [
    {
      "key": "bank_denom",
      "value": "\"unibi\"",
      "index": true
    },
    {
      "key": "creator",
      "value": "\"nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl\"",
      "index": true
    },
    {
      "key": "erc20_contract_address",
      "value": "\"0x7D4B7B8CA7E1a24928Bb96D59249c7a5bd1DfBe6\"",
      "index": true
    },
    {
      "key": "is_made_from_coin",
      "value": "true",
      "index": true
    }
  ]
}
```



</details>

## Convert native coins to ERC-20

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
RECIPIENT=0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8

nibid tx evm convert-coin-to-evm \
$RECIPIENT \
1000unibi \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

<details>

<summary>Output</summary>

```json
{
  "type": "eth.evm.v1.EventConvertCoinToEvm",
  "attributes": [
    {
      "key": "bank_coin",
      "value": "{\"denom\":\"unibi\",\"amount\":\"1000\"}",
      "index": true
    },
    {
      "key": "erc20_contract_address",
      "value": "\"0x7D4B7B8CA7E1a24928Bb96D59249c7a5bd1DfBe6\"",
      "index": true
    },
    {
      "key": "sender",
      "value": "\"nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl\"",
      "index": true
    },
    {
      "key": "to_eth_addr",
      "value": "\"0xFaF227daD0b91C2dEBD41daE71C959EA4f95f8F8\"",
      "index": true
    }
  ]
}
```



</details>
