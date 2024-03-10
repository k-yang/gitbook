# CLI Setup

{% hint style="info" %}
I personally use an M1 MacBook pro (darwin/arm64), but these commands should work on Unix environments.
{% endhint %}

```bash
curl -s https://get.nibiru.fi/@v1.0.2! | bash

nibid config chain-id cataclysm-1 && \
nibid config broadcast-mode sync && \
nibid config node "https://nibiru.rpc.kjnodes.com:443" && \
nibid config keyring-backend test && \
nibid config output json
```

```bash
# in your .bashrc or .zshrc file
alias tx="jq -rcs '.[0].txhash' | { read txhash; sleep 5; nibid q tx \$txhash | jq }"
```
