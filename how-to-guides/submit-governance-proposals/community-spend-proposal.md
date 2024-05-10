---
description: Disburses funds from the on-chain community reserve
---

# Community spend proposal

## Draft the proposal

```bash
RECIPIENT=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4
AMOUNT=100000000

cat << EOF | jq | tee draft_proposal.json
{
 "messages": [
  {
   "@type": "/cosmos.distribution.v1beta1.MsgCommunityPoolSpend",
   "authority": "nibi10d07y265gmmuvt4z0w9aw880jnsr700jd8hulq",
   "recipient": "$RECIPIENT",
   "amount": [
    {
     "amount": "$AMOUNT",
     "denom": "unibi"
    }
   ]
  }
 ],
 "metadata": "",
 "deposit": "20000000000unibi",
 "title": "<enter title here>",
 "summary": "<enter summary here>"
}
EOF
```

The `authority` has to be the gov module account, which on Nibiru Chain is `nibi10d07y265gmmuvt4z0w9aw880jnsr700jd8hulq`.&#x20;

## Submit the proposal

```bash
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

nibid tx gov submit-proposal ./draft_proposal.json \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--sign-mode amino-json \
--yes | tx
```
