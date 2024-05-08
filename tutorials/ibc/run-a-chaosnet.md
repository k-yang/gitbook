---
description: >-
  Chaosnet is a local Docker cluster of two blockchain networks connected by a
  Hermes relayer
---

# Run a chaosnet

## Prerequisites

* You must have [Docker](https://docs.docker.com/engine/install/) insalled on your host computer
* You must have the `make` command installed
  * macOS has a [Homebrew package](https://formulae.brew.sh/formula/make)

## Clone the repository

```bash
git clone https://github.com/NibiruChain/nibiru.git
cd nibiru/
```

## Start chaosnet

```bash
make chaosnet
```

This command takes a while because it builds a lot of Docker images. You should have a Docker compose network with:

* two blockchains
  * one validator for each blockchain
  * one pricefeeder for each validator
* one relayer
