---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# CLI Setup

{% hint style="info" %}
I personally use an M1 MacBook pro (darwin/arm64), but these commands should work on any Unix environment.
{% endhint %}

## Download the binary

```bash
curl -s https://get.nibiru.fi/@v1.2.0! | bash
```

## Configure the CLI

### Mainnet (cataclysm-1)

```
nibid config chain-id cataclysm-1 && \
nibid config broadcast-mode sync && \
nibid config node "https://nibiru.rpc.kjnodes.com:443" && \
nibid config keyring-backend test && \
nibid config output json
```

### nibiru-testnet-1

```bash
nibid config chain-id nibiru-testnet-1 && \
nibid config broadcast-mode sync && \
nibid config node "https://rpc.testnet-1.nibiru.fi:443" && \
nibid config keyring-backend test && \
nibid config output json
```

## (Optional) Query TX Command Alias

Create a command alias to help with querying tx hashes after tx submission. You'll come across this command in following tutorials.&#x20;

```bash
# in your .bashrc or .zshrc file
alias tx="jq -rcs '.[0].txhash' | { read txhash; sleep 5; nibid q tx \$txhash | jq }"
```
