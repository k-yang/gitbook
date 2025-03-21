# Claim vested tokens

## Claim tokens

Use the token vesting contract address from the previous guide, or given to you by the contract deployer.

```bash
CONTRACT_ADDRESS=nibi1unyuj8qnmygvzuex3dwmg9yzt9alhvyeat0uu0jedg2wj33efl5qlplsk6
RECIPIENT=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

cat <<EOF | jq | tee claim.json
{
    "claim": {}
}
EOF

nibid tx wasm execute \
$CONTRACT_ADDRESS \
"$(cat claim.json)" \
--from $RECIPIENT \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```
