# IBC Transfer

First, start a [chaosnet](run-a-chaosnet.md)

## SSH into nibiru-0

Open up a terminal and run the following command

```bash
docker compose -f ./contrib/docker-compose/docker-compose-chaosnet.yml exec -it nibiru-0 /bin/ash
```

## Transfer IBC tokens

```bash
nibid tx ibc-transfer transfer transfer \
channel-0 \
nibi18mxturdh0mjw032c3zslgkw63cukkl4q5skk8g \
100000000unibi \
--from validator \
--fees 5000unibi \
--yes | jq
```

## SSH into nibiru-1

Open up a second terminal and run the following command

```bash
docker compose -f ./contrib/docker-compose/docker-compose-chaosnet.yml exec -it nibiru-1 /bin/ash
```

Update the cli config in the nibiru-1 container, because we use different ports to avoid confusion:

```bash
nibid config node "http://localhost:36657"
```

## Query account balance

```bash
nibid q bank balances $(nibid keys show validator -a) | jq
```

The output should resemble the following:

```json
{
  "balances": [
    {
      "denom": "ibc/9BEE732637B12723D26E365D19CCB624587CE6254799EEE7C5F77B587BD677B0",
      "amount": "100000000"
    },
    {
      "denom": "unibi",
      "amount": "9999100000000"
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "0"
  }
}
```

The `ibc/9BEE732637B12723D26E365D19CCB624587CE6254799EEE7C5F77B587BD677B0` denom is from the nibiru-0 blockchain.

## Trace an IBC denom

It's possible to figure out where an IBC denom came from

```bash
nibid q ibc-transfer denom-trace \
ibc/9BEE732637B12723D26E365D19CCB624587CE6254799EEE7C5F77B587BD677B0 | jq
```

Output:

```json
{
  "denom_trace": {
    "path": "transfer/channel-0",
    "base_denom": "unibi"
  }
}
```

It's telling us that the IBC denom came from `port: transfer` and `channel: channel-0`.
