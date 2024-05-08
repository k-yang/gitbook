# Bank Transfer

```bash
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4
RECIPIENT=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
AMOUNT=500000000

nibid tx bank send \
$FROM \
$RECIPIENT \
${AMOUNT}unibi \
--fees 5000unibi \
--sign-mode amino-json \
--note "memo" \
--yes | tx
```
