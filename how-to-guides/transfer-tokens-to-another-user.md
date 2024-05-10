---
description: Simple bank transfer
---

# Transfer tokens to another user

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
RECIPIENT=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4
AMOUNT=500000000

nibid tx bank send \
$FROM \
$RECIPIENT \
${AMOUNT}unibi \
--fees 5000unibi \
--note "memo" \
--yes | tx
```
