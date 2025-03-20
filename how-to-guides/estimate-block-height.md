---
description: How to estimate the block height at a given timestamp
---

# Estimate block height

## Download Agoric's block estimator

```bash
git clone https://github.com/Agoric/estimator.git && \
cd estimator
```

## Estimate a datetime's block height

```bash
go run main.go \
-date "May 13, 2024 12:00PM GMT" \
-votingPeriod 48h \
-rpc https://rpc.nibiru.fi:443 \
-samples 10000
```

10000 samples should be good enough to get a close enough estimate. It should take less than 10 minutes to run.&#x20;

## Estimate a block height's datetime

```bash
go run main.go \
-height 100 \
-rpc https://rpc.nibiru.fi:443 \
-samples 10000
```
