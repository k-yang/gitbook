---
description: Toggles on-chain inflation
---

# Toggle inflation

## Draft proposal

```bash
cat << EOF | jq | tee draft_proposal.json
{
  "title": "Turn on inflation",
  "description": "Test turning off/on inflation params",
  "changes": [
    {
      "subspace": "inflation",
      "key": "InflationEnabled",
      "value": true
    }
  ],
  "deposit": "20000000000unibi"
}
EOF
```

Use `false` to turn off inflation.&#x20;

## Submit proposal

We use the legacy proposal handler because the `x/inflation` module hasn't upgraded to the new way of handling params yet.&#x20;

```bash
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

nibid tx gov submit-legacy-proposal \
param-change \
./draft_proposal.json \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```
