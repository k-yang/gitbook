# IBC Transfer

## Nibiru -> Osmosis

```bash
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4
RECIPIENT=osmo1gc6vpl9j0ty8tkt53787zps9ezc70kj8cfg435

nibid tx ibc-transfer transfer \
transfer channel-0 \
$RECIPIENT \
1000000unibi \
--from $FROM \
--gas-prices 0.025unibi \
--sign-mode amino-json \
--yes | tx
```
