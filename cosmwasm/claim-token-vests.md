# Claim Token Vests

## Claim Tokens

```bash
CONTRACT_ADDRESS=nibi1uumtewjjrpmhn5yukvwfpcr83g7xtzwd0lz9a96rdkqprfajfz3q6hmfx4
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm execute \
$CONTRACT_ADDRESS \
'{"claim":{}}' \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

## Query Vest

```bash
CONTRACT_ADDRESS=nibi1uumtewjjrpmhn5yukvwfpcr83g7xtzwd0lz9a96rdkqprfajfz3q6hmfx4
USER_ADDRESS=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid query wasm contract-state smart \
$CONTRACT_ADDRESS \
"{\"vesting_account\":{\"address\":\"$USER_ADDRESS\"}}" | jq
```
