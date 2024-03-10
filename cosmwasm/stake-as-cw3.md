# Stake as CW3

## Create Proposal

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4

nibid tx staking delegate \
nibivaloper140l6y2gp3gxvay6qtn70re7z2s0gn57zj8exm0 \
69420unibi \
--from $MULTISIG \
--fees 5000unibi \
--generate-only | \
jq ".body.messages[0]" | \
tee out/tmp_msg.json

# encode it as b64
nibid build-stargate-wasm "$(cat out/tmp_msg.json)" | \
jq | \
tee out/tmp_msg_b64.json

# create proposal
nibid tx wasm execute \
$MULTISIG \
"$(cat out/propose.json)" \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--from $FROM \
--yes | tx

# save the proposal_id for the next step
```

## Vote on Proposal

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4
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

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4
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
--yes
```

## Query a Proposal

```bash
MULTISIG=nibi1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrqugq26k

nibid q wasm contract-state smart $MULTISIG \
'{"proposal": {"proposal_id": 1}}' | \
jq ".data"
```
