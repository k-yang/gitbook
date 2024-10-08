# Query all delegations

```bash
DELEGATOR=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

nibid q staking delegations $DELEGATOR
```



## Query all delegations sorted by balance in descending order

```bash
DELEGATOR=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

nibid q staking delegations \
$DELEGATOR | \
jq '.delegation_responses | map({validator_address: .delegation.validator_address, balance: .balance.amount}) | sort_by(.balance) | reverse'
```
