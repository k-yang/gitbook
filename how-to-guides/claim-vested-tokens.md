# Claim vested tokens

## Claim tokens

```bash
cat <<EOF | jq | tee claim.json
{
    "claim": {}
}
EOF

CONTRACT_ADDRESS=nibi14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9ssa9gcs
RECIPIENT=nibi1lq3ktemm9rhpu0je850rnlrny752v6yu9wurws

nibid tx wasm execute \
$CONTRACT_ADDRESS \
"$(cat claim.json)" \
--from $RECIPIENT \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

## Query vested tokens

```bash
CONTRACT_ADDRESS=nibi14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9ssa9gcs
RECIPIENT=nibi1lq3ktemm9rhpu0je850rnlrny752v6yu9wurws

nibid query wasm contract-state smart \
$CONTRACT_ADDRESS \
"{\"vesting_account\":{\"address\":\"$RECIPIENT\"}}" | jq
```
