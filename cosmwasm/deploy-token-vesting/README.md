# Deploy Token Vesting

## Download contract

```bash
curl -Lso core_token_vesting_v2.wasm \
https://github.com/NibiruChain/cw-nibiru/releases/download/latest/core_token_vesting_v2.wasm
```

## Store contract

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm store core_token_vesting_v2.wasm \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx

# note the code id for the next step
```

## Instantiate contract

```bash
CODE_ID=1

cat << EOF | jq | tee instantiate.json
{
    "admin": "nibi1l8dxzwz9d4peazcqjclnkj2mhvtj7mpnkqx85mg0ndrlhwrnh7gskkzg0v",
    "managers": ["nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4","nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"]
}
EOF

nibid tx wasm instantiate $CODE_ID "$(cat instantiate.json)" \
--admin nibi1l8dxzwz9d4peazcqjclnkj2mhvtj7mpnkqx85mg0ndrlhwrnh7gskkzg0v \
--from nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl \
--label "Token vesting contract label" \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--amount 100unibi \
--yes | tx

# note the --amount flag, because you can't change it once sent
# note the contract address in the response
```

## Reward user

```bash
START_TIME=$(date -u -j -f "%Y-%m-%d %H:%M:%S" "2024-03-13 00:00:00" +%s)
CLIFF_TIME=$(date -u -j -f "%Y-%m-%d %H:%M:%S" "2024-03-13 00:00:00" +%s)
END_TIME=$(date -u -j -f "%Y-%m-%d %H:%M:%S" "2024-09-12 00:00:00" +%s)

CLIFF_AMOUNT=0
VESTING_AMOUNT=100
RECIPIENT=nibi1lq3ktemm9rhpu0je850rnlrny752v6yu9wurws

cat << EOF | jq | tee reward_user.json
{
	"reward_users": {
		"rewards": [
			{
				"user_address": "$RECIPIENT",
				"vesting_amount": "$VESTING_AMOUNT",
				"cliff_amount": "$CLIFF_AMOUNT"
			}
		],
		"vesting_schedule": {
			"linear_vesting_with_cliff": {
				"start_time": "$START_TIME",
				"cliff_time": "$CLIFF_TIME",
				"end_time": "$END_TIME"
			}
		}
	}
}
EOF

nibid tx wasm execute \
nibi14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9ssa9gcs \
"$(cat reward_user.json)" \
--from nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4 \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```





