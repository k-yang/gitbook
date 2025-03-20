---
description: Deploy a simple counter contract that can increment and reset
---

# Counter

Requires running a [localnet](../run-locally.md).

## Prerequisites

Install Rust and Cargo (via [rustup](https://rustup.rs/)).

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Install some Rust and Cargo dependencies

```bash
rustup target add wasm32-unknown-unknown

cargo install cargo-generate --features vendored-openssl
cargo install cargo-run-script
```

## Clone the repo

```bash
cargo generate --git https://github.com/CosmWasm/cw-template.git --name tutorial

cd tutorial/
```

## Compile the contracts

To produce a simple, unoptimized build, run the following:

```bash
cargo wasm
```

It should produce a compiled `*.wasm` file in the `artifacts/` directory.

## Upload the contract

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm store artifacts/tutorial-aarch64.wasm \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

## Instantiate a contract instance

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl

nibid tx wasm instantiate 1 \
'{"count": 0}' \
--label "label" \
--admin $FROM \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

<details>

<summary>Example output</summary>

```json
{
  "height": "6405",
  "txhash": "D098A824C15463FBF1AFDA0F72EA358592A0B3560F63E47989C7B0C6ADD5871B",
  "codespace": "",
  "code": 0,
  "data": "12750A302F636F736D7761736D2E7761736D2E76312E4D7367496E7374616E7469617465436F6E7472616374526573706F6E736512410A3F6E696269317375686766357376687534757372757276787A6C676E35346B73786D6E38676C6A61726A7478716E617076386B6A6E70346E7273306766617365",
  "raw_log": "[{\"msg_index\":0,\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"/cosmwasm.wasm.v1.MsgInstantiateContract\"},{\"key\":\"sender\",\"value\":\"nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl\"},{\"key\":\"module\",\"value\":\"wasm\"}]},{\"type\":\"instantiate\",\"attributes\":[{\"key\":\"_contract_address\",\"value\":\"nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase\"},{\"key\":\"code_id\",\"value\":\"1\"}]},{\"type\":\"wasm\",\"attributes\":[{\"key\":\"_contract_address\",\"value\":\"nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase\"},{\"key\":\"method\",\"value\":\"instantiate\"},{\"key\":\"owner\",\"value\":\"nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl\"},{\"key\":\"count\",\"value\":\"0\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "log": "",
      "events": [
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "/cosmwasm.wasm.v1.MsgInstantiateContract"
            },
            {
              "key": "sender",
              "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"
            },
            {
              "key": "module",
              "value": "wasm"
            }
          ]
        },
        {
          "type": "instantiate",
          "attributes": [
            {
              "key": "_contract_address",
              "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase"
            },
            {
              "key": "code_id",
              "value": "1"
            }
          ]
        },
        {
          "type": "wasm",
          "attributes": [
            {
              "key": "_contract_address",
              "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase"
            },
            {
              "key": "method",
              "value": "instantiate"
            },
            {
              "key": "owner",
              "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"
            },
            {
              "key": "count",
              "value": "0"
            }
          ]
        }
      ]
    }
  ],
  "info": "",
  "gas_wanted": "207394",
  "gas_used": "157071",
  "tx": {
    "@type": "/cosmos.tx.v1beta1.Tx",
    "body": {
      "messages": [
        {
          "@type": "/cosmwasm.wasm.v1.MsgInstantiateContract",
          "sender": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "admin": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "code_id": "1",
          "label": "label",
          "msg": {
            "count": 0
          },
          "funds": []
        }
      ],
      "memo": "",
      "timeout_height": "0",
      "extension_options": [],
      "non_critical_extension_options": []
    },
    "auth_info": {
      "signer_infos": [
        {
          "public_key": {
            "@type": "/cosmos.crypto.secp256k1.PubKey",
            "key": "AvzwBOriY8sVwEXrXf1gXanhT9imlfWeUWLQ8pMxrRsg"
          },
          "mode_info": {
            "single": {
              "mode": "SIGN_MODE_DIRECT"
            }
          },
          "sequence": "4"
        }
      ],
      "fee": {
        "amount": [
          {
            "denom": "unibi",
            "amount": "5185"
          }
        ],
        "gas_limit": "207394",
        "payer": "",
        "granter": ""
      },
      "tip": null
    },
    "signatures": [
      "gMgWL8Tt4OOkhsJ6NWYaDlxRb5JZ7ZCTDlV/hlhcseA6PaejCU2yEWMNv9s0W9g7uvBqXtSpN3yu96G/J9x4Zg=="
    ]
  },
  "timestamp": "2024-05-10T17:48:57Z",
  "events": [
    {
      "type": "coin_spent",
      "attributes": [
        {
          "key": "spender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        },
        {
          "key": "amount",
          "value": "5185unibi",
          "index": true
        }
      ]
    },
    {
      "type": "coin_received",
      "attributes": [
        {
          "key": "receiver",
          "value": "nibi17xpfvakm2amg962yls6f84z3kell8c5l8u8ezw",
          "index": true
        },
        {
          "key": "amount",
          "value": "5185unibi",
          "index": true
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "nibi17xpfvakm2amg962yls6f84z3kell8c5l8u8ezw",
          "index": true
        },
        {
          "key": "sender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        },
        {
          "key": "amount",
          "value": "5185unibi",
          "index": true
        }
      ]
    },
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "fee",
          "value": "5185unibi",
          "index": true
        },
        {
          "key": "fee_payer",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "acc_seq",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl/4",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "signature",
          "value": "gMgWL8Tt4OOkhsJ6NWYaDlxRb5JZ7ZCTDlV/hlhcseA6PaejCU2yEWMNv9s0W9g7uvBqXtSpN3yu96G/J9x4Zg==",
          "index": true
        }
      ]
    },
    {
      "type": "message",
      "attributes": [
        {
          "key": "action",
          "value": "/cosmwasm.wasm.v1.MsgInstantiateContract",
          "index": true
        },
        {
          "key": "sender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        },
        {
          "key": "module",
          "value": "wasm",
          "index": true
        }
      ]
    },
    {
      "type": "instantiate",
      "attributes": [
        {
          "key": "_contract_address",
          "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase",
          "index": true
        },
        {
          "key": "code_id",
          "value": "1",
          "index": true
        }
      ]
    },
    {
      "type": "wasm",
      "attributes": [
        {
          "key": "_contract_address",
          "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase",
          "index": true
        },
        {
          "key": "method",
          "value": "instantiate",
          "index": true
        },
        {
          "key": "owner",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        },
        {
          "key": "count",
          "value": "0",
          "index": true
        }
      ]
    }
  ]
}
```

</details>

Save the contract address for the next step.

## Execute the contract

To increment the counter in the contract, run the following:

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
CONTRACT_ADDRESS=nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase

nibid tx wasm execute \
$CONTRACT_ADDRESS \
'{"increment": {}}' \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

<details>

<summary>Example output</summary>

```json
{
  "height": "6480",
  "txhash": "70682D0A43219EF4B968F91890EA1B8BA4BBC24912838231F3B2800BE14B7031",
  "codespace": "",
  "code": 0,
  "data": "122E0A2C2F636F736D7761736D2E7761736D2E76312E4D736745786563757465436F6E7472616374526573706F6E7365",
  "raw_log": "[{\"msg_index\":0,\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"/cosmwasm.wasm.v1.MsgExecuteContract\"},{\"key\":\"sender\",\"value\":\"nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl\"},{\"key\":\"module\",\"value\":\"wasm\"}]},{\"type\":\"execute\",\"attributes\":[{\"key\":\"_contract_address\",\"value\":\"nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase\"}]},{\"type\":\"wasm\",\"attributes\":[{\"key\":\"_contract_address\",\"value\":\"nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase\"},{\"key\":\"action\",\"value\":\"increment\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "log": "",
      "events": [
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "/cosmwasm.wasm.v1.MsgExecuteContract"
            },
            {
              "key": "sender",
              "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl"
            },
            {
              "key": "module",
              "value": "wasm"
            }
          ]
        },
        {
          "type": "execute",
          "attributes": [
            {
              "key": "_contract_address",
              "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase"
            }
          ]
        },
        {
          "type": "wasm",
          "attributes": [
            {
              "key": "_contract_address",
              "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase"
            },
            {
              "key": "action",
              "value": "increment"
            }
          ]
        }
      ]
    }
  ],
  "info": "",
  "gas_wanted": "150273",
  "gas_used": "118990",
  "tx": {
    "@type": "/cosmos.tx.v1beta1.Tx",
    "body": {
      "messages": [
        {
          "@type": "/cosmwasm.wasm.v1.MsgExecuteContract",
          "sender": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "contract": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase",
          "msg": {
            "increment": {}
          },
          "funds": []
        }
      ],
      "memo": "",
      "timeout_height": "0",
      "extension_options": [],
      "non_critical_extension_options": []
    },
    "auth_info": {
      "signer_infos": [
        {
          "public_key": {
            "@type": "/cosmos.crypto.secp256k1.PubKey",
            "key": "AvzwBOriY8sVwEXrXf1gXanhT9imlfWeUWLQ8pMxrRsg"
          },
          "mode_info": {
            "single": {
              "mode": "SIGN_MODE_DIRECT"
            }
          },
          "sequence": "5"
        }
      ],
      "fee": {
        "amount": [
          {
            "denom": "unibi",
            "amount": "3757"
          }
        ],
        "gas_limit": "150273",
        "payer": "",
        "granter": ""
      },
      "tip": null
    },
    "signatures": [
      "DLFLTETW6DmnTuknLkMzCXCS0usNuqeaf+NcsB+w6rRzqucUd2mr3oG+CkjQGhLmFbkWF6vPL8uK/YbSF84zOA=="
    ]
  },
  "timestamp": "2024-05-10T17:50:14Z",
  "events": [
    {
      "type": "coin_spent",
      "attributes": [
        {
          "key": "spender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        },
        {
          "key": "amount",
          "value": "3757unibi",
          "index": true
        }
      ]
    },
    {
      "type": "coin_received",
      "attributes": [
        {
          "key": "receiver",
          "value": "nibi17xpfvakm2amg962yls6f84z3kell8c5l8u8ezw",
          "index": true
        },
        {
          "key": "amount",
          "value": "3757unibi",
          "index": true
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "nibi17xpfvakm2amg962yls6f84z3kell8c5l8u8ezw",
          "index": true
        },
        {
          "key": "sender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        },
        {
          "key": "amount",
          "value": "3757unibi",
          "index": true
        }
      ]
    },
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "fee",
          "value": "3757unibi",
          "index": true
        },
        {
          "key": "fee_payer",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "acc_seq",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl/5",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "signature",
          "value": "DLFLTETW6DmnTuknLkMzCXCS0usNuqeaf+NcsB+w6rRzqucUd2mr3oG+CkjQGhLmFbkWF6vPL8uK/YbSF84zOA==",
          "index": true
        }
      ]
    },
    {
      "type": "message",
      "attributes": [
        {
          "key": "action",
          "value": "/cosmwasm.wasm.v1.MsgExecuteContract",
          "index": true
        },
        {
          "key": "sender",
          "value": "nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl",
          "index": true
        },
        {
          "key": "module",
          "value": "wasm",
          "index": true
        }
      ]
    },
    {
      "type": "execute",
      "attributes": [
        {
          "key": "_contract_address",
          "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase",
          "index": true
        }
      ]
    },
    {
      "type": "wasm",
      "attributes": [
        {
          "key": "_contract_address",
          "value": "nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase",
          "index": true
        },
        {
          "key": "action",
          "value": "increment",
          "index": true
        }
      ]
    }
  ]
}
```

</details>

## Query the contract

To query the counter, run the following:

```bash
CONTRACT_ADDRESS=nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase

nibid q wasm contract-state smart \
$CONTRACT_ADDRESS \
'{"get_count": {}}' | jq ".data"
```

<details>

<summary>Example output</summary>

```json
{
  "count": 1
}
```

</details>

## Reset the counter

```bash
FROM=nibi1zaavvzxez0elundtn32qnk9lkm8kmcsz44g7xl
CONTRACT_ADDRESS=nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase

nibid tx wasm execute \
$CONTRACT_ADDRESS \
'{"reset": {"count": 0}}' \
--from $FROM \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 0.025unibi \
--yes | tx
```

## Query the counter (again)

```bash
CONTRACT_ADDRESS=nibi1suhgf5svhu4usrurvxzlgn54ksxmn8gljarjtxqnapv8kjnp4nrs0gfase

nibid q wasm contract-state smart \
$CONTRACT_ADDRESS \
'{"get_count": {}}' | jq ".data"
```

<details>

<summary>Example output</summary>

```json
{
  "count": 0
}
```

</details>
