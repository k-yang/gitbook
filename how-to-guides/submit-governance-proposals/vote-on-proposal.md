# Vote on proposal

## Query proposals

### Query all proposals

```bash
nibid q gov proposals | jq
```

### Query proposals by status

```bash
nibid q gov proposals --status (DepositPeriod|VotingPeriod|Passed|Rejected) | jq
```

Example output:

```json
{
  "proposals": [
    {
      "id": "1",
      "messages": [
        {
          "@type": "/cosmos.gov.v1.MsgExecLegacyContent",
          "content": {
            "@type": "/cosmos.upgrade.v1beta1.SoftwareUpgradeProposal",
            "title": "v1.1.0",
            "description": "Upgrade to v1.1.0",
            "plan": {
              "name": "v1.1.0",
              "time": "0001-01-01T00:00:00Z",
              "height": "48579",
              "info": "{\n  \"binaries\": {\n    \"linux/amd64\": \"https://github.com/NibiruChain/nibiru/releases/download/v1.1.0/nibid_1.1.0_linux_amd64.tar.gz\",\n    \"linux/arm64\": \"https://github.com/NibiruChain/nibiru/releases/download/v1.1.0/nibid_1.1.0_linux_arm64.tar.gz\",\n    \"docker\": \"ghcr.io/nibiruchain/nibiru:1.1.0\"\n  }\n}",
              "upgraded_client_state": null
            }
          },
          "authority": "nibi10d07y265gmmuvt4z0w9aw880jnsr700jd8hulq"
        }
      ],
      "status": "PROPOSAL_STATUS_PASSED",
      "final_tally_result": {
        "yes_count": "40600000",
        "abstain_count": "0",
        "no_count": "0",
        "no_with_veto_count": "0"
      },
      "submit_time": "2023-12-21T17:41:48.975625159Z",
      "deposit_end_time": "2023-12-21T18:41:48.975625159Z",
      "total_deposit": [
        {
          "denom": "unibi",
          "amount": "10000000"
        }
      ],
      "voting_start_time": "2023-12-21T17:41:48.975625159Z",
      "voting_end_time": "2023-12-21T18:41:48.975625159Z",
      "metadata": "",
      "title": "v1.1.0",
      "summary": "Upgrade to v1.1.0",
      "proposer": "nibi1lq3ktemm9rhpu0je850rnlrny752v6yu9wurws"
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "0"
  }
}
```

### Query proposal by id

```bash
PROPOSAL_ID=1

nibid q gov proposal $PROPOSAL_ID | jq
```

Example output:

```json
{
  "id": "1",
  "messages": [
    {
      "@type": "/cosmos.gov.v1.MsgExecLegacyContent",
      "content": {
        "@type": "/cosmos.upgrade.v1beta1.SoftwareUpgradeProposal",
        "title": "v1.1.0",
        "description": "Upgrade to v1.1.0",
        "plan": {
          "name": "v1.1.0",
          "time": "0001-01-01T00:00:00Z",
          "height": "48579",
          "info": "{\n  \"binaries\": {\n    \"linux/amd64\": \"https://github.com/NibiruChain/nibiru/releases/download/v1.1.0/nibid_1.1.0_linux_amd64.tar.gz\",\n    \"linux/arm64\": \"https://github.com/NibiruChain/nibiru/releases/download/v1.1.0/nibid_1.1.0_linux_arm64.tar.gz\",\n    \"docker\": \"ghcr.io/nibiruchain/nibiru:1.1.0\"\n  }\n}",
          "upgraded_client_state": null
        }
      },
      "authority": "nibi10d07y265gmmuvt4z0w9aw880jnsr700jd8hulq"
    }
  ],
  "status": "PROPOSAL_STATUS_PASSED",
  "final_tally_result": {
    "yes_count": "40600000",
    "abstain_count": "0",
    "no_count": "0",
    "no_with_veto_count": "0"
  },
  "submit_time": "2023-12-21T17:41:48.975625159Z",
  "deposit_end_time": "2023-12-21T18:41:48.975625159Z",
  "total_deposit": [
    {
      "denom": "unibi",
      "amount": "10000000"
    }
  ],
  "voting_start_time": "2023-12-21T17:41:48.975625159Z",
  "voting_end_time": "2023-12-21T18:41:48.975625159Z",
  "metadata": "",
  "title": "v1.1.0",
  "summary": "Upgrade to v1.1.0",
  "proposer": "nibi1lq3ktemm9rhpu0je850rnlrny752v6yu9wurws"
}

```

## Vote on proposal

Use the proposal id from the previous step

```bash
FROM=nibi1lq3ktemm9rhpu0je850rnlrny752v6yu9wurws
PROPOSAL_ID=1

nibid tx gov vote $PROPOSAL_ID yes \
--from $FROM \
--gas 200000 \
--gas-prices 0.025unibi \
--yes | tx
```
