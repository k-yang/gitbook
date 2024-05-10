---
description: >-
  Software upgrade proposals halt the chain and force validators to upgrade
  their binary versions
---

# Software upgrade proposal

## Create the proposal

```bash
FROM=nibi1gc6vpl9j0ty8tkt53787zps9ezc70kj88hluw4
VERSION=1.2.0

cat << EOF | jq | tee upgrade_info.json
{
    "binaries": {
        "linux/amd64": "https://github.com/NibiruChain/nibiru/releases/download/v${VERSION}/nibid_${VERSION}_linux_amd64.tar.gz",
        "linux/arm64": "https://github.com/NibiruChain/nibiru/releases/download/v${VERSION}/nibid_${VERSION}_linux_arm64.tar.gz",
        "docker": "ghcr.io/nibiruchain/nibiru:${VERSION}"
    }
}
EOF
```

## Submit the proposal

```bash
HEIGHT=4804662

nibid tx gov submit-legacy-proposal \
software-upgrade \
"v${VERSION}" \
--title "v${VERSION}" \
--description "Upgrade to v${VERSION}" \
--upgrade-height $HEIGHT \
--upgrade-info "$(cat upgrade_info.json)" \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--deposit 20000000000unibi \
--no-validate \
--sign-mode amino-json \
--yes | tx
```

Check out [estimate-block-height.md](../estimate-block-height.md "mention") on selecting a proper block height.
