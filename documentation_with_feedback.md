
### Comments and Suggested Updates

> **Feedback**  
> - Sepolia links are invalid; update links to Alchemy: `https://www.alchemy.com/chain-connect/endpoints/alchemy-sepolia` (requires API key).  
> - Mention use of Foundry:  
    ```bash
    curl -L https://foundry.paradigm.xyz | bash
    source ~/.bashrc
    foundryup
    ```
> - Add link for Avail wallet setup and app ID creation: [Avail Wallet Guide](https://docs.availproject.org/docs/end-user-guide/app-id).  
> - Provide link for Avail tokens: [Avail Faucet](https://faucet.avail.tools/).

---

### Key Account Creation

Create four essential accounts with private keys:

- **Admin** (contract upgrade authority)
- **Batcher** (publishes Sequencer data to L1)
- **Proposer** (publishes L2 results to L1)
- **Sequencer** (signs blocks on the p2p network)

> **Note:**  
> Consider reordering commands for clarity. Mention usage of `cast wallet` in the `contracts-bedrock` package for generating keys.

```bash
cd ~/optimism/packages/contracts-bedrock
echo "Admin:"
cast wallet new
echo "Batcher:"
cast wallet new
echo "Proposer:"
cast wallet new
echo "Sequencer:"
cast wallet new
```

The output should be similar to:

```plaintext
Admin:
Successfully created new keypair.
Address:     0xc4A01194958DE0D90A876e8A5fc9D7B530072148
Private key: 0xb8e39bd94a210e410c4024e1cc91014de45a5eb1e42f3aa99a368b5a5ac19b45
...
```

> **Important:**  
> Securely record these key details. Fund **Admin**, **Proposer**, and **Batcher** with Sepolia ETH (e.g., 0.5 ETH for Admin, 0.2 for Proposer, 0.1 for Batcher).  

> **Production Note:**  
> Use secure hardware for production key management as `cast wallet` is not production-ready.

---

### Network Configuration and Setup

Configure chain settings after repository setup in the `contracts-bedrock` package.

> **Feedback**  
> - Instruct user to copy `.envrc.example` into `.envrc` within `contracts-bedrock`.  
> - Update the example to match order of commands for user roles.

```bash
cd ~/optimism/packages/contracts-bedrock
direnv allow .
source .envrc
```

> **Link:**  
> Sample `.envrc` example: [Github Link](https://github.com/ethereum-optimism/optimism/blob/develop/.envrc.example)

---

### Core Contract Deployment

Update `deploy-config/getting-started.json` in `contracts-bedrock` for deployment.

> **Feedback**  
> Mention `getting_started.json` file is generated in the `deploy-config` folder.

```bash
cd deploy-config
nano getting-started.json
```

Add configuration for AltDA usage:

```json
{
  "useAltDA": true,
  "daCommitmentType": "GenericCommitment",
  "daChallengeWindow": 160,
  "daResolveWindow": 160,
  "daBondSize": 1000000,
  "daResolverRefundPercentage": 0
}
```

---

### Example Configuration

> **Reference Configuration:**  
> The following is an example config for reference purposes.

```json
{
  "l1StartingBlockTag": "0x2c9f156ae1cc024817b920b41c102b23b5a7526e16220c517078341f9890e8bd",
  "l1ChainID": 11155111,
  "l2ChainID": 11155420,
  "l2BlockTime": 2,
  "l1BlockTime": 12,
  ...
}
```

Each comment and update in the document has been highlighted to indicate feedback and required changes.
