---
description: Runs a localnet without destroying the local keyring
---

# Run Localnet

```bash
curl -s https://get.nibiru.fi/@v1.0.2! | bash
nibid tendermint unsafe-reset-all
rm ~/.nibid/config/genesis.json
rm -rf ~/.nibid/config/gentx/

CHAIN=nibiru-localnet-0
nibid init $CHAIN --chain-id=$CHAIN
nibid config chain-id $CHAIN
nibid config node "http://localhost:26657"

# x/gov voting_period
cat ~/.nibid/config/genesis.json | jq '.app_state.gov.params.voting_period = "15s"' > ~/.nibid/config/tmp_genesis.json
mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

# x/sudo root
cat ~/.nibid/config/genesis.json | jq '.app_state.sudo.sudoers.root = "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"' > ~/.nibid/config/tmp_genesis.json
mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

cat ~/.nibid/config/genesis.json | jq '.app_state.sudo.sudoers.contracts = ["nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"]' > ~/.nibid/config/tmp_genesis.json
mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

# x/epochs day
cat ~/.nibid/config/genesis.json | jq '.app_state.epochs.epochs = [{
          "identifier": "day",
          "start_time": "0001-01-01T00:00:00Z",
          "duration": "5s",
          "current_epoch": "0",
          "current_epoch_start_time": "0001-01-01T00:00:00Z",
          "epoch_counting_started": false,
          "current_epoch_start_height": "0"
        }]' > ~/.nibid/config/tmp_genesis.json
mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

# x/inflation
cat ~/.nibid/config/genesis.json | jq '.app_state.inflation.params.inflation_enabled = true' > ~/.nibid/config/tmp_genesis.json
mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

nibid add-genesis-account nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl 10000000000000000000unibi
nibid genesis gentx validator 9000000000unibi --chain-id $CHAIN
nibid genesis collect-gentxs

nibid start
```
