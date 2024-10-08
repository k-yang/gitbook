# Withdraw all staking rewards



```bash
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

nibid tx distribution withdraw-all-rewards \
--from $FROM \
--fees 5000unibi \
--sign-mode amino-json \
--yes | jq
```
