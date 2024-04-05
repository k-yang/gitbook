# Submit Governance Proposal

## Software Upgrade

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

nibid tx gov submit-legacy-proposal \
software-upgrade \
"v${VERSION}" \
--title "v${VERSION}" \
--description "Upgrade to v${VERSION}" \
--upgrade-height 4804662 \
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
