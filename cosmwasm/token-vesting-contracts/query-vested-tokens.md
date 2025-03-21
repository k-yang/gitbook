# Query vested tokens

## Query vested tokens

```bash
CONTRACT_ADDRESS=nibi1unyuj8qnmygvzuex3dwmg9yzt9alhvyeat0uu0jedg2wj33efl5qlplsk6
RECIPIENT=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

nibid query wasm contract-state smart \
$CONTRACT_ADDRESS \
"{\"vesting_account\":{\"address\":\"$RECIPIENT\"}}" | jq
```

The output should resemble the following:

```json
{
  "data": {
    "address": "nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4",
    "vestings": [
      {
        "master_address": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
        "vesting_denom": {
          "native": "unibi"
        },
        "vesting_amount": "100000000",
        "vesting_schedule": {
          "linear_vesting_with_cliff": {
            "start_time": "1715126400",
            "end_time": "1725753600",
            "cliff_time": "1717804800",
            "vesting_amount": "100000000",
            "cliff_amount": "25000000"
          }
        },
        "vested_amount": "0",
        "claimable_amount": "0"
      }
    ]
  }
}
```
