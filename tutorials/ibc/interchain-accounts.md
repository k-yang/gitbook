# Interchain Accounts

First, start a [chaosnet](run-a-chaosnet.md)

## SSH into nibiru-0

Open up a terminal and run the following command

```bash
docker compose -f ./contrib/docker-compose/docker-compose-chaosnet.yml exec -it nibiru-0 /bin/ash
```

## Register an interchain account

```bash
# on nibiru-0
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx interchain-accounts controller \
register \
connection-0 \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | jq
```

## Query the interchain account address

```bash
# on nibiru-0
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid q interchain-accounts controller \
interchain-account \
$FROM \
connection-0 | jq
```

Output:

```json
{
  "address": "nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a"
}
```

## SSH into nibiru-1

Open up a second terminal and run the following command

```bash
docker compose -f ./contrib/docker-compose/docker-compose-chaosnet.yml exec -it nibiru-1 /bin/ash
```

Update the cli config in the nibiru-1 container, because we use different ports to avoid confusion:

```bash
# on nibiru-1
nibid config node "http://localhost:36657"
```

## Fund the interchain account

```bash
# on nibiru-1
INTERCHAIN_ADDRESS=nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a

nibid tx bank send \
$(nibid keys show validator -a) \
$INTERCHAIN_ADDRESS \
100000000unibi \
--yes | jq
```

## Query interchain account balance

```bash
# on nibiru-1
INTERCHAIN_ADDRESS=nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a

nibid q bank balances $INTERCHAIN_ADDRESS | jq
```

Output:

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

## Query validators

```bash
# on nibiru-1
nibid q staking validators | \
jq '[.validators[] | select(.jailed == false) | .operator_address] | sort | .[]'
```

Output:

```bash
"nibivaloper18mxturdh0mjw032c3zslgkw63cukkl4qatcdn4"
```

## Generate packet data

```bash
# on nibiru-1
INTERCHAIN_ADDRESS=nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a
VALIDATOR=nibivaloper18mxturdh0mjw032c3zslgkw63cukkl4qatcdn4

cat << EOF | jq | tee msg_delegate.json
{
  "@type":"/cosmos.staking.v1beta1.MsgDelegate",
  "delegator_address":"$INTERCHAIN_ADDRESS",
  "validator_address":"$VALIDATOR",
  "amount": {
    "denom": "unibi",
    "amount": "500000"
  }
}
EOF

nibid tx interchain-accounts host \
generate-packet-data \
"$(cat msg_delegate.json)" \
--encoding proto3 | jq
```

Output:

```json
{
  "type": "TYPE_EXECUTE_TX",
  "data": "Cq4BCiMvY29zbW9zLnN0YWtpbmcudjFiZXRhMS5Nc2dEZWxlZ2F0ZRKGAQo/bmliaTEzMGFybmR3NzRsNW5td2RnZGFoOTQwbTdmOTB0dmE4MDkzaGpndnpseG5jZXA1ank3YXZzMGE2ZzdhEjJuaWJpdmFsb3BlcjE4bXh0dXJkaDBtancwMzJjM3pzbGdrdzYzY3Vra2w0cWF0Y2RuNBoPCgV1bmliaRIGNTAwMDAw",
  "memo": ""
}
```

## Send packet from controller chain

Return to the terminal tab that's SSHed into nibiru-0

```bash
# on nibiru-0

cat << EOF | jq | tee packet.json
{
  "type": "TYPE_EXECUTE_TX",
  "data": "Cq4BCiMvY29zbW9zLnN0YWtpbmcudjFiZXRhMS5Nc2dEZWxlZ2F0ZRKGAQo/bmliaTEzMGFybmR3NzRsNW5td2RnZGFoOTQwbTdmOTB0dmE4MDkzaGpndnpseG5jZXA1ank3YXZzMGE2ZzdhEjJuaWJpdmFsb3BlcjE4bXh0dXJkaDBtancwMzJjM3pzbGdrdzYzY3Vra2w0cWF0Y2RuNBoPCgV1bmliaRIGNTAwMDAw",
  "memo": ""
}
EOF

FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx interchain-accounts controller send-tx \
connection-0 \
packet.json \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | jq
```

## Query delegations on host chain

```bash
# on nibiru-1
INTERCHAIN_ADDRESS=nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a
VALIDATOR=nibivaloper18mxturdh0mjw032c3zslgkw63cukkl4qatcdn4

nibid q staking delegation $INTERCHAIN_ADDRESS $VALIDATOR | jq
```

Output:

```bash
{
  "delegation": {
    "delegator_address": "nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a",
    "validator_address": "nibivaloper18mxturdh0mjw032c3zslgkw63cukkl4qatcdn4",
    "shares": "500000.000000000000000000"
  },
  "balance": {
    "denom": "unibi",
    "amount": "500000"
  }
}
```

## Query packet data

```bash
# on nibiru-1
nibid q interchain-accounts host packet-events channel-1 1
```

Output:

```yaml
- tx
	- fee: 0unibi
	- fee_payer: nibi18mxturdh0mjw032c3zslgkw63cukkl4q5skk8g
- tx
	- acc_seq: nibi18mxturdh0mjw032c3zslgkw63cukkl4q5skk8g/12
- tx
	- signature: bNyPNw9Kg5LXz8Dxpm1uYq1obHBhhMt5ec4ZQpN3wyBmiaONeECszq3yXBTaR3ahd0NcDZGA2FTS/uaMSV03ow==
- message
	- action: /ibc.core.client.v1.MsgUpdateClient
	- sender: nibi18mxturdh0mjw032c3zslgkw63cukkl4q5skk8g
- update_client
	- client_id: 07-tendermint-0
	- client_type: 07-tendermint
	- consensus_height: 0-1307
	- consensus_heights: 0-1307
	- header: 0a262f6962632e6c69676874636c69656e74732e74656e6465726d696e742e76312e48656164657212e5060ad5040a98030a02080b12116e69626972752d6c6f63616c6e65742d30189b0a220c08cda4efb10610d5c2e3cd012a480a2034e4c54d6978a6abb29bf983620693bf0b22a8afd26969dda6a8eb71eba2547a122408011220d69073a6416554ccb148771dcc09a3cd00903910361caff90971776c6a6e59fd32203046b1ebc1c9a28ae8508b24c42b8a086ac2bef9c1cdbc35be75bd134d29a0c53a20e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b8554220ae2089aab42292f9c92547fc532ee413751d1dd949a79cdff7c5c3efef7de83e4a20ae2089aab42292f9c92547fc532ee413751d1dd949a79cdff7c5c3efef7de83e5220048091bc7ddc283f77bfbf91d73c44da58c3df8a9cbc867405d8b7f3daada22f5a20fdb11d002a5066080d7fc27dd003e0596c71b533428304aedd1fa0df9604e7be622086e6f52fad1be3c00945b5e32452aa3f9c31260c54755300f7cf0aaca447b4a96a20e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b85572146db5d415d93b096e972aff7116b650d2b251d3aa12b701089b0a1a480a200fffeb5470c3069bc14d3c24c4ec1baf23945d9d3d62a53be3d8295a84c7818a1224080112204a8649adca098d67afeefba23ae48d3a19c274982fc94bb5422bd3d740b956732268080212146db5d415d93b096e972aff7116b650d2b251d3aa1a0c08cea4efb10610da9eeed801224015300076f942a8e23ac65547fd5b6be6407578b665f75ec5a6510558f37d8b7c36ceb50fe047b6239c9ec0c1d8d3e2f05a7ba85337125ae915cb305d05c0c20d1281010a3d0a146db5d415d93b096e972aff7116b650d2b251d3aa12220a203154d0e99a8ef0f1e52123bf2995463d76d1e4f78c44be7edc7fa595f0cabc5a188407123d0a146db5d415d93b096e972aff7116b650d2b251d3aa12220a203154d0e99a8ef0f1e52123bf2995463d76d1e4f78c44be7edc7fa595f0cabc5a1884071884071a0310ef082281010a3d0a146db5d415d93b096e972aff7116b650d2b251d3aa12220a203154d0e99a8ef0f1e52123bf2995463d76d1e4f78c44be7edc7fa595f0cabc5a188407123d0a146db5d415d93b096e972aff7116b650d2b251d3aa12220a203154d0e99a8ef0f1e52123bf2995463d76d1e4f78c44be7edc7fa595f0cabc5a188407188407
- message
	- module: ibc_client
- message
	- action: /ibc.core.channel.v1.MsgRecvPacket
	- sender: nibi18mxturdh0mjw032c3zslgkw63cukkl4q5skk8g
- recv_packet
	- packet_data: {"data":"Cq4BCiMvY29zbW9zLnN0YWtpbmcudjFiZXRhMS5Nc2dEZWxlZ2F0ZRKGAQo/bmliaTEzMGFybmR3NzRsNW5td2RnZGFoOTQwbTdmOTB0dmE4MDkzaGpndnpseG5jZXA1ank3YXZzMGE2ZzdhEjJuaWJpdmFsb3BlcjE4bXh0dXJkaDBtancwMzJjM3pzbGdrdzYzY3Vra2w0cWF0Y2RuNBoPCgV1bmliaRIGNTAwMDAw","memo":"","type":"TYPE_EXECUTE_TX"}
	- packet_data_hex: 7b2264617461223a224371344243694d765932397a6257397a4c6e4e3059577470626d6375646a46695a5852684d53354e633264455a57786c5a3246305a524b4741516f2f626d6c696154457a4d474679626d52334e7a52734e5735746432526e5a47466f4f5451776254646d4f544230646d45344d446b7a6147706e646e70736547356a5a584131616e6b3359585a7a4d4745325a7a6468456a4a7561574a70646d46736233426c636a45346258683064584a6b61444274616e63774d7a4a6a4d33707a62476472647a597a593356726132773063574630593252754e426f5043675631626d6c69615249474e5441774d444177222c226d656d6f223a22222c2274797065223a22545950455f455845435554455f5458227d
	- packet_timeout_height: 0-0
	- packet_timeout_timestamp: 1715197092406843167
	- packet_sequence: 2
	- packet_src_port: icacontroller-nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
	- packet_src_channel: channel-1
	- packet_dst_port: icahost
	- packet_dst_channel: channel-1
	- packet_channel_ordering: ORDER_ORDERED
	- packet_connection: connection-0
	- connection_id: connection-0
- message
	- module: ibc_channel
- coin_spent
	- spender: nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a
	- amount: 500000unibi
- coin_received
	- receiver: nibi1fl48vsnmsdzcv85q5d2q4z5ajdha8yu3z7dksy
	- amount: 500000unibi
- delegate
	- validator: nibivaloper18mxturdh0mjw032c3zslgkw63cukkl4qatcdn4
	- delegator: nibi130arndw74l5nmwdgdah940m7f90tva8093hjgvzlxncep5jy7avs0a6g7a
	- amount: 500000unibi
	- new_shares: 500000.000000000000000000
- ics27_packet
	- module: interchainaccounts
	- host_channel_id: channel-1
	- success: true
- write_acknowledgement
	- packet_data: {"data":"Cq4BCiMvY29zbW9zLnN0YWtpbmcudjFiZXRhMS5Nc2dEZWxlZ2F0ZRKGAQo/bmliaTEzMGFybmR3NzRsNW5td2RnZGFoOTQwbTdmOTB0dmE4MDkzaGpndnpseG5jZXA1ank3YXZzMGE2ZzdhEjJuaWJpdmFsb3BlcjE4bXh0dXJkaDBtancwMzJjM3pzbGdrdzYzY3Vra2w0cWF0Y2RuNBoPCgV1bmliaRIGNTAwMDAw","memo":"","type":"TYPE_EXECUTE_TX"}
	- packet_data_hex: 7b2264617461223a224371344243694d765932397a6257397a4c6e4e3059577470626d6375646a46695a5852684d53354e633264455a57786c5a3246305a524b4741516f2f626d6c696154457a4d474679626d52334e7a52734e5735746432526e5a47466f4f5451776254646d4f544230646d45344d446b7a6147706e646e70736547356a5a584131616e6b3359585a7a4d4745325a7a6468456a4a7561574a70646d46736233426c636a45346258683064584a6b61444274616e63774d7a4a6a4d33707a62476472647a597a593356726132773063574630593252754e426f5043675631626d6c69615249474e5441774d444177222c226d656d6f223a22222c2274797065223a22545950455f455845435554455f5458227d
	- packet_timeout_height: 0-0
	- packet_timeout_timestamp: 1715197092406843167
	- packet_sequence: 2
	- packet_src_port: icacontroller-nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
	- packet_src_channel: channel-1
	- packet_dst_port: icahost
	- packet_dst_channel: channel-1
	- packet_ack: {"result":"Ei0KKy9jb3Ntb3Muc3Rha2luZy52MWJldGExLk1zZ0RlbGVnYXRlUmVzcG9uc2U="}
	- packet_ack_hex: 7b22726573756c74223a224569304b4b79396a62334e7462334d756333526861326c755a7935324d574a6c644745784c6b317a5a30526c6247566e5958526c556d567a634739756332553d227d
	- packet_connection: connection-0
	- connection_id: connection-0
- message
	- module: ibc_channel
```
