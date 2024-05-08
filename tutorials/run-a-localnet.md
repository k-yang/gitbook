---
description: 'Goal: Run a single-node blockchain on your local comput'
---

# Run a Localnet

## Download the binary

```bash
curl -s https://get.nibiru.fi/@v1.2.0! | bash
```

## Remove any pre-existing state

Only necessary if you have pre-existing state in your `~/.nibid` folder, i.e. if you've run a local chain before.

{% hint style="danger" %}
Never run these commands on your mainnet nodes, or else you'll lose data.
{% endhint %}

```bash
nibid tendermint unsafe-reset-all
rm ~/.nibid/config/genesis.json
rm -rf ~/.nibid/config/gentx/
```

## Initialize the home directory

```bash
CHAIN=nibiru-localnet-0

nibid init $CHAIN --chain-id=$CHAIN
```

## Configure the CLI

```bash
CHAIN=nibiru-localnet-0

nibid config chain-id $CHAIN && \
nibid config broadcast-mode sync && \
nibid config node "http://localhost:26657" && \
nibid config keyring-backend test && \
nibid config output json
```

{% hint style="warning" %}
We use the **test** keyring-backend for local development purposes, but it's not recommended for production.&#x20;
{% endhint %}

## Create a wallet

Creates a new address in your local keyring

```bash
nibid keys add tutorial | jq

# sample output
{
  "name": "tutorial",
  "type": "local",
  "address": "nibi1n84pu3he9z3yf8cgwhy2xakx6a9ysv58ate3em",
  "pubkey": "{\"@type\":\"/cosmos.crypto.secp256k1.PubKey\",\"key\":\"A8BqmOx2Jjc6dPJQsuVK60qAYPa9ancLaX6HNZYVvNj+\"}",
  "mnemonic": "method west genius either liquid rely tobacco tobacco scorpion high across town shell category hurry satoshi sibling file decide mimic alarm toy rival govern"
}
```

## Configure x/sudo state

Required because we don't allow empty sudo addresses in the genesis state. Use the address from the step above.

<pre class="language-bash"><code class="lang-bash"><strong>cat ~/.nibid/config/genesis.json | jq '.app_state.sudo.sudoers.root = "nibi1n84pu3he9z3yf8cgwhy2xakx6a9ysv58ate3em"' > ~/.nibid/config/tmp_genesis.json
</strong>mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

cat ~/.nibid/config/genesis.json | jq '.app_state.sudo.sudoers.contracts = ["nibi1n84pu3he9z3yf8cgwhy2xakx6a9ysv58ate3em"]' > ~/.nibid/config/tmp_genesis.json
mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json
</code></pre>

## Setup genesis accounts

```bash
CHAIN=nibiru-localnet-0
ADDRESS=nibi1n84pu3he9z3yf8cgwhy2xakx6a9ysv58ate3em

nibid add-genesis-account $ADDRESS 10000000000000000000unibi
nibid genesis gentx tutorial 9000000000unibi --chain-id $CHAIN
nibid genesis collect-gentxs
```

## Start the chain

```bash
nibid start
```
