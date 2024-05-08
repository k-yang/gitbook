---
description: >-
  Goal: deploy a cw3-flex-multisig contract backed by the cw4-group created in
  the previous tutorial
---

# Deploy cw3-flex-multisig

## Upload cw3 contract

```bash
curl -Lso cw3_flex_multisig.wasm \
https://github.com/NibiruChain/cw-nibiru/releases/download/latest/cw3_flex_multisig.wasm

FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm store cw3_flex_multisig.wasm \
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
      "value": "b56a880d4c67d9f353f549b502256f73159f89b50aa6dae683948e117efa4792",
      "index": true
    },
    {
      "key": "code_id",
      "value": "2",
      "index": true
    }
  ]
}
```

## Instantiate cw3

Use the code id from the previous step, and the cw4-group contract address from the previous tutorial

```bash
CODE_ID=2
CW4_GROUP_ADDRESS=nibi14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9ssa9gcs
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

cat << EOF | jq | tee cw3_instantiate_args.json
{
    "group_addr": "$CW4_GROUP_ADDRESS",
    "max_voting_period": {
        "time": 86400
    },
    "threshold": {
        "absolute_count": {
            "weight": 1
        }
    }
}
EOF

nibid tx wasm instantiate $CODE_ID \
"$(cat cw3_instantiate_args.json)" \
--admin "$FROM" \
--label "cw3 flex multisig" \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

Take note of the contract address in the output for the next tutorial, e.g.

```json
{
  "type": "instantiate",
  "attributes": [
    {
      "key": "_contract_address",
      "value": "nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k",
      "index": true
    },
    {
      "key": "code_id",
      "value": "2",
      "index": true
    }
  ]
}
```
