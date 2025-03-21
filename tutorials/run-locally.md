---
description: Run a single-node blockchain locally
---

# Run locally

## Download the binary

```bash
curl -s https://get.nibiru.fi/@v2.1.0 | bash
```

{% hint style="info" %}
You can check [https://github.com/NibiruChain/nibiru/releases](https://github.com/NibiruChain/nibiru/releases) for the latest release version.
{% endhint %}

## \[Optional] Remove old local state

Only required if you have pre-existing state in your `~/.nibid` folder (if you've run a local chain before).

{% hint style="danger" %}
Never run these commands on mainnet nodes, or else you'll lose data.
{% endhint %}

```bash
nibid tendermint unsafe-reset-all
rm ~/.nibid/config/genesis.json
rm -rf ~/.nibid/config/gentx/
```

## Initialize the config files

```bash
CHAIN=nibiru-localnet-0

nibid init $CHAIN --chain-id=$CHAIN

nibid config chain-id $CHAIN && \
nibid config broadcast-mode sync && \
nibid config node "http://localhost:26657" && \
nibid config keyring-backend test && \
nibid config output json
```

{% hint style="warning" %}
We use the **test** keyring-backend for local development purposes, but it's not recommended for production.&#x20;
{% endhint %}

## \[Optional] Create a wallet

### Option 1: Create a new mnemonic

Creates a new address in your local keyring

```bash
nibid keys add validator | jq

# sample output
{
  "name": "validator",
  "type": "local",
  "address": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
  "pubkey": "{\"@type\":\"/cosmos.crypto.secp256k1.PubKey\",\"key\":\"A8BqmOx2Jjc6dPJQsuVK60qAYPa9ancLaX6HNZYVvNj+\"}",
  "mnemonic": "guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host"
}
```

### Option 2: Import an existing mnemonic

```bash
nibid keys add validator --recover
```

You can input the following mnemonic:

```
guard cream sadness conduct invite crumble clock pudding hole grit liar hotel maid produce squeeze return argue turtle know drive eight casino maze host
```

{% hint style="warning" %}
The mnemonics that appear on this page are no longer safe and should not be used in production.
{% endhint %}

## Configure the genesis state

We require some custom genesis state to be set in order for the chain to start.

<pre class="language-bash"><code class="lang-bash"><strong>cat ~/.nibid/config/genesis.json | jq '.app_state.sudo.sudoers.root = "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"' > ~/.nibid/config/tmp_genesis.json
</strong>mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

cat ~/.nibid/config/genesis.json | jq '.app_state.sudo.sudoers.contracts = ["nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"]' > ~/.nibid/config/tmp_genesis.json
mv ~/.nibid/config/tmp_genesis.json ~/.nibid/config/genesis.json

CHAIN=nibiru-localnet-0
ADDRESS=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid add-genesis-account $ADDRESS 10000000000000000000unibi
nibid genesis gentx validator 9000000000unibi --chain-id $CHAIN
nibid genesis collect-gentxs
</code></pre>

## Start the chain

```bash
nibid start
```
