---
description: Update gov params
---

# Update gov params

## Fetch current gov params

```bash
nibid q gov params | jq ".params"
```

Output:

```json
{
  "min_deposit": [
    {
      "denom": "unibi",
      "amount": "20000000000"
    }
  ],
  "max_deposit_period": "3600s",
  "voting_period": "3600s",
  "quorum": "0.334000000000000000",
  "threshold": "0.500000000000000000",
  "veto_threshold": "0.334000000000000000",
  "min_initial_deposit_ratio": "0.000000000000000000",
  "burn_vote_quorum": false,
  "burn_proposal_deposit_prevote": false,
  "burn_vote_veto": true
}
```

## Draft proposal

```bash
cat << EOF | jq | tee draft_proposal.json
{
	"messages": [{
		"@type": "/cosmos.gov.v1.MsgUpdateParams",
		"authority": "nibi10d07y265gmmuvt4z0w9aw880jnsr700jd8hulq",
		"params": {
     		    "min_deposit": [{
			"denom": "unibi",
			"amount": "20000000000"
		    }],
		    "max_deposit_period": "172800s",
		    "voting_period": "172800s",
		    "quorum": "0.334000000000000000",
		    "threshold": "0.500000000000000000",
		    "veto_threshold": "0.334000000000000000",
		    "min_initial_deposit_ratio": "0.000000000000000000",
		    "burn_vote_quorum": false,
		    "burn_proposal_deposit_prevote": false,
		    "burn_vote_veto": true
		}
	}],
	"metadata": "",
	"deposit": "20000000000unibi",
	"title": "Increase governance proposal minimum deposit to 20K NIBI",
	"summary": "The prior threshold of 10 NIBI is too low. A low governance deposit threshold may lead to spam proposals as NIBI becomes available."
}
EOF
```

## Submit proposal

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
