---
description: 'Goal: deploy a token vesting contract'
---

# Deploy token vesting contract

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
```

## Instantiate contract

Use the code is from the previous step.&#x20;

The `amount` cannot be changed after instantiation.

```bash
CODE_ID=3
AMOUNT=100000000unibi
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
ADMIN=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl # admin and from can be different

cat << EOF | jq | tee instantiate.json
{
    "admin": "$ADMIN",
    "managers": ["$ADMIN"]
}
EOF

nibid tx wasm instantiate $CODE_ID "$(cat instantiate.json)" \
--admin $ADMIN \
--from $FROM \
--label "contract label" \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--amount $AMOUNT \
--yes | tx
```

Save the contract address from the response.

## Prepare the reward user message

```bash
START_TIME=$(date -u -j -f "%Y-%m-%d %H:%M:%S" "2024-05-08 00:00:00" +%s)
CLIFF_TIME=$(date -u -j -f "%Y-%m-%d %H:%M:%S" "2024-06-08 00:00:00" +%s)
END_TIME=$(date -u -j -f "%Y-%m-%d %H:%M:%S" "2024-09-08 00:00:00" +%s)

CLIFF_AMOUNT=25000000
VESTING_AMOUNT=100000000
RECIPIENT=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

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
```

## Execute the reward user message

Use the contract address from the previous step

```bash
CONTRACT_ADDR=nibi1unyuj8qnmygvzuex3dwmg9yzt9alhvyeat0uu0jedg2wj33efl5qlplsk6
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm execute \
$CONTRACT_ADDR \
"$(cat reward_user.json)" \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```
