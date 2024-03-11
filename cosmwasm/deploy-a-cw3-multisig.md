---
description: CW3 is a spec for on-chain multisigs, similar to Gnosis Safes on Ethereum.
---

# Deploy a CW3 Multisig

{% hint style="info" %}
A CW3 flex multisig contract uses a CW4 group contract as the backing privileged member set. The CW4 group contract is responsible for maintaining membership of who can approve proposals, and the CW3 flex multisig contract handles creating, voting, and executing proposals. Proposals are essentially wrapped messages that execute against the AppChain’s msg server.
{% endhint %}

## Upload CW4 Group Contract

```bash
curl -Lso cw4_group.wasm \
https://github.com/CosmWasm/cw-plus/releases/download/v1.1.2/cw4_group.wasm

FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm store cw4_group.wasm \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx

# take note of the code id for the next step
```

## Instantiate the CW4 Group Contract

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
CODE_ID=1

cat << EOF | jq | tee cw4_group_instantiate_args.json
{
    "admin": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
    "members": [
        {
            "addr": "nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4",
            "weight": 1
        },
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

# save the contract address for the next step
# e.g. nibi14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9ssa9gcs
```

## Upload CW3 Flex Multisig Contract

{% hint style="info" %}
We compile our own cw3 flex multisig with the stargate feature enabled, but feel free to use the contracts from the official CosmWasm repo
{% endhint %}

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

# save the code id for the next step
```

## Instantiate CW3 Flex Multisig

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

# save the contract address, which is your multisig address
# e.g. nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
```

