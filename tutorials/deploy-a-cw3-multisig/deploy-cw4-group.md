---
description: 'Goal: deploy a cw4 group contract which backs a cw3-flex-multisig contract'
---

# Deploy cw4-group

Requires a [localnet](../run-a-localnet.md) to be running.

## Download contract bytecode

```bash
curl -Lso cw4_group.wasm \
https://github.com/CosmWasm/cw-plus/releases/download/v1.1.2/cw4_group.wasm
```

## Upload cw4 contract

Stores the contract bytecode onchain so that it can be instantiated (next step).&#x20;

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm store cw4_group.wasm \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

Take note of the code id for the next step, e.g.

```json
{
  "type": "store_code",
  "attributes": [
    {
      "key": "code_checksum",
      "value": "dd2216f1114fc68bc4c043701b02e55ce3e5598cdeb616985388215a400db277",
      "index": true
    },
    {
      "key": "code_id",
      "value": "1",
      "index": true
    }
  ]
}
```

## Instantiate cw4 contract

A contract instance is created from bytecode stored onchain.&#x20;

Use the code id from the previous step.

```bash
CODE_ID=1 # code id from last step
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

cat << EOF | jq | tee cw4_group_instantiate_args.json
{
    "admin": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
    "members": [
        {
            "addr": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
            "weight": 1
        }
    ]
}
EOF

nibid tx wasm instantiate $CODE_ID \
"$(cat cw4_group_instantiate_args.json)" \
--admin "$FROM" \
--label "cw4 group" \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

Save the contract address for cw3 contract instantiation, e.g.

```json
{
  "type": "instantiate",
  "attributes": [
    {
      "key": "_contract_address",
      "value": "nibi14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9ssa9gcs",
      "index": true
    },
    {
      "key": "code_id",
      "value": "1",
      "index": true
    }
  ]
}
```
