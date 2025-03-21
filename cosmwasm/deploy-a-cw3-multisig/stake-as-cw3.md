---
description: 'Goal: stake as the cw3 multisig contract'
---

# Stake as cw3

## Fund the cw3 contract

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx bank send \
$FROM \
$MULTISIG \
100000000unibi \
--fees 5000unibi \
--yes | tx
```

## (Optional) Verify funds were transferred

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k

nibid q bank balances $MULTISIG | jq
```

You should get a response similar to the following:

```json
{
  "balances": [
    {
      "denom": "unibi",
      "amount": "100000000"
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "0"
  }
}
```

## Query list of validators

We need a validator to stake to

```bash
nibid q staking validators | \
jq '[.validators[] | select(.jailed == false) | .operator_address] | sort | .[]'

# nibivaloper1zaavvzxez0elundtn32qnk9lkm8kmcszuwx9jz
```

## Create cw3 proposal

Every action executed by a cw3 multisig must first be proposed and voted on.&#x20;

Select a validator address from the previous step.

### Create base64 encoded stargate msg

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
VALIDATOR=nibivaloper1zaavvzxez0elundtn32qnk9lkm8kmcszuwx9jz

nibid tx staking delegate \
$VALIDATOR \
69420unibi \
--from $MULTISIG \
--fees 5000unibi \
--generate-only | \
jq ".body.messages[0]" | \
tee tmp_msg.json

# encode it as b64
nibid build-stargate-wasm "$(cat tmp_msg.json)" | \
jq | \
tee tmp_msg_b64.json
```

### Upload proposal

```bash
cat << EOF | jq | tee propose.json
{
  "propose": {
    "title": "Stake to $VALIDATOR",
    "description": "Stake 69420unibi to $VALIDATOR",
    "msgs": $(cat tmp_msg_b64.json | jq -s '.')
  }
}
EOF

# create proposal
nibid tx wasm execute \
$MULTISIG \
"$(cat propose.json)" \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--from $FROM \
--yes | tx
```

You should get a proposal id in the event logs, e.g.

```json
{
  "type": "wasm",
  "attributes": [
    {
      "key": "_contract_address",
      "value": "nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k",
      "index": true
    },
    {
      "key": "action",
      "value": "propose",
      "index": true
    },
    {
      "key": "sender",
      "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
      "index": true
    },
    {
      "key": "proposal_id",
      "value": "1",
      "index": true
    },
    {
      "key": "status",
      "value": "Passed",
      "index": true
    }
  ]
}
```

## (Optional) Vote on Proposal

There's no need to vote on the cw3 proposal if the proposal already passed (it should be if you're following this tutorial and only have one member in the cw4-group).

Use the proposal id from the previous step.

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
PROPOSAL_ID=1

cat << EOF | jq | tee vote.json
{
	 "vote": {
		"proposal_id": $PROPOSAL_ID,
		"vote": "yes"
	}
}
EOF

nibid tx wasm execute $MULTISIG "$(cat vote.json)" \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

## Execute the Proposal

Send the execution tx and complete the proposal. All msgs in the proposal will be sent as though the sender is the cw3 multisig contract.&#x20;

Use the proposal id from the previous step.

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
PROPOSAL_ID=1

cat << EOF | jq | tee execute.json
{
	 "execute": {
		"proposal_id": $PROPOSAL_ID
	}
}
EOF

nibid tx wasm execute $MULTISIG "$(cat execute.json)" \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

The output should resemble the following:

```json
{
  "type": "delegate",
  "attributes": [
    {
      "key": "validator",
      "value": "nibivaloper1zaavvzxez0elundtn32qnk9lkm8kmcszuwx9jz",
      "index": true
    },
    {
      "key": "delegator",
      "value": "nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k",
      "index": true
    },
    {
      "key": "amount",
      "value": "69420unibi",
      "index": true
    },
    {
      "key": "new_shares",
      "value": "69420.000000000000000000",
      "index": true
    }
  ]
}
```

## Query validator delegations

Verify that the delegation went through

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
VALIDATOR=nibivaloper1zaavvzxez0elundtn32qnk9lkm8kmcszuwx9jz

nibid q staking delegation \
$MULTISIG \
$VALIDATOR | jq
```

If everything was successful, then the output should resemble

```json
{
  "delegation": {
    "delegator_address": "nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k",
    "validator_address": "nibivaloper1zaavvzxez0elundtn32qnk9lkm8kmcszuwx9jz",
    "shares": "69420.000000000000000000"
  },
  "balance": {
    "denom": "unibi",
    "amount": "69420"
  }
}
```

## Query the proposal

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k

nibid q wasm contract-state smart $MULTISIG \
'{"proposal": {"proposal_id": 1}}' | \
jq ".data"
```

The output should resemble the following:

```json
{
  "id": 1,
  "title": "Stake to nibivaloper1zaavvzxez0elundtn32qnk9lkm8kmcszuwx9jz",
  "description": "Stake 69420unibi to nibivaloper1zaavvzxez0elundtn32qnk9lkm8kmcszuwx9jz",
  "msgs": [
    {
      "stargate": {
        "type_url": "/cosmos.staking.v1beta1.MsgDelegate",
        "value": "Cj9uaWJpMW5jNXRhdGFmdjZleXE3bGxrcjJndjUwZmY5ZTIybW5mNzBxZ2psdjczN2t0bXQ0ZXN3cnF1Z3EyNmsSMm5pYml2YWxvcGVyMXphYXZ2enhlejBlbHVuZHRuMzJxbms5bGttOGttY3N6dXd4OWp6Gg4KBXVuaWJpEgU2OTQyMA=="
      }
    }
  ],
  "status": "executed",
  "expires": {
    "at_time": "1715278767750315000"
  },
  "threshold": {
    "absolute_count": {
      "weight": 1,
      "total_weight": 2
    }
  },
  "proposer": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
  "deposit": null
}
```
