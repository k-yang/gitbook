# Tokenfactory commands

## Create denom

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
nibid tx tf create-denom foo \
--from $FROM \
--gas auto \
--gas-prices 0.025unibi \
--gas-adjustment 1.5 \
--yes | tx
```

## Query denom

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
nibid q tf denoms $FROM | jq
```

## Query denom info

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
nibid q tf denom-info tf/$FROM/foo | jq
```

## Query total balances

```bash
nibid q bank total | jq
```

## Mint denom

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
TO=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
ADMIN=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx tf mint \
100tf/$ADMIN/foo \
--mint-to $TO \
--from $FROM \
--gas auto \
--gas-prices 0.025unibi \
--gas-adjustment 1.5 \
--yes | tx
```
