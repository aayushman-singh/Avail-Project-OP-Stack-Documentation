
### Mismatch in Commands and Documentation

> **Comment:**  
> - Go version update: `1.20` → `1.21.6`  
> - Node version update: `16` → `20`

---

# Install Git
```bash
sudo apt install -y git curl make jq
```
> **Note:**  
> `wget` is also needed. 

---

# Install Go
> **Comment:**  
> Recommended update for Go installation instructions to Go version `1.21.6`.  

- **Download and install Go 1.21.6**  
    ```bash
    wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz
    ```
- **Extract the downloaded tarball**  
    ```bash
    tar xvzf go1.21.6.linux-amd64.tar.gz
    ```
- **Move Go to the correct directory**  
    ```bash
    sudo mv go /usr/local
    ```
- **Set the GOROOT and update the PATH in .bashrc**  
    ```bash
    echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
    echo 'export GOPATH=$HOME/go' >> ~/.bashrc
    echo 'export PATH=$GOROOT/bin:$GOPATH/bin:$PATH' >> ~/.bashrc
    ```
- **Reload .bashrc to apply changes**  
    ```bash
    source ~/.bashrc
    ```
- **Verify Go installation**  
    ```bash
    go version
    ```

---

# Install Node.js
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs npm
```

---

# Install Pnpm
```bash
sudo npm install -g pnpm
```

---

# Install Make
```bash
sudo apt install -y make
```

---

# Install jq
```bash
sudo apt install -y jq
```

---

# Install direnv
```bash
sudo apt install -y direnv
```

---

### Avail DA Server Setup
> **Note:**  
> It’s essential to keep the Avail DA server running for subsequent steps.

1. **Clone and Navigate to Avail DA Server**  
    ```bash
    git clone https://github.com/availproject/avail-alt-da-server.git
    cd avail-alt-da-server
    ```
2. **Build Avail DA Server**  
    ```bash
    make da-server
    ```
3. **Run DA Sidecar**  
    ```bash
    ./bin/avail-da-server ./cmd/avail --addr=localhost --port=8000 --avail.rpc=<WS url to an avail node> --avail.seed=<> --avail.appid=<>
    ```
    > **Tip:**  
    > WS URLs can be found [here](https://docs.availproject.org/docs/networks) or run your own DA node (more complex setup).

---

### Running via Docker
1. Copy `.env.example` to `.env` and fill in necessary values.
2. Run the following commands:
    ```bash
    docker-compose build
    docker-compose up
    ```

---

### Build the Adapter Source
1. **Clone and Navigate to Avail Adapter**  
    ```bash
    git clone https://github.com/ethereum-optimism/optimism.git
    cd optimism
    git fetch --tag --all
    git checkout v1.9.1
    git submodule update --init --recursive
    ```
2. **Install Modules**  
    ```bash
    pnpm install
    ```
3. **Compile Necessary Packages**  
    ```bash
    make op-node op-batcher op-proposer
    pnpm build
    ```

---

### Build the Optimism Geth Source
1. **Clone and Navigate to op-geth**  
    ```bash
    git clone https://github.com/ethereum-optimism/op-geth.git
    cd op-geth
    git fetch --tag --all
    git checkout v1.101408.0
    ```
2. **Compile op-geth**  
    ```bash
    make geth
    ```

--- 




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

 "l1StartingBlockTag": "0x0b2b81474a22fc1122bbb3a465985c5cb40dfd8ef18bfe4fc0a9fa47e775d692",

  "l1ChainID": 11155111,
  "l2ChainID": 42069,
  "l2BlockTime": 2,
  "l1BlockTime": 12,

  "maxSequencerDrift": 600,
  "sequencerWindowSize": 3600,
  "channelTimeout": 300,

  "p2pSequencerAddress": "0xd34514056DBE102dF5c24DAf3e78701b502F34c7",
  "batchInboxAddress": "0xff00000000000000000000000000000000042069",
  "batchSenderAddress": "0x4eD53FeB5b06c60368490683e2b317d7C20eC41E",

  "l2OutputOracleSubmissionInterval": 120,
  "l2OutputOracleStartingBlockNumber": 0,
  "l2OutputOracleStartingTimestamp": 1729571268,

  "l2OutputOracleProposer": "0x7d32557e4e79F494836037aD622AaB60125D8323",
  "l2OutputOracleChallenger": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",

  "finalizationPeriodSeconds": 12,

  "proxyAdminOwner": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",
  "baseFeeVaultRecipient": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",
  "l1FeeVaultRecipient": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",
  "sequencerFeeVaultRecipient": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",
  "finalSystemOwner": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",
  "superchainConfigGuardian": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",

  "baseFeeVaultMinimumWithdrawalAmount": "0x8ac7230489e80000",
  "l1FeeVaultMinimumWithdrawalAmount": "0x8ac7230489e80000",
  "sequencerFeeVaultMinimumWithdrawalAmount": "0x8ac7230489e80000",
  "baseFeeVaultWithdrawalNetwork": 0,
  "l1FeeVaultWithdrawalNetwork": 0,
  "sequencerFeeVaultWithdrawalNetwork": 0,

  "gasPriceOracleOverhead": 0,
  "gasPriceOracleScalar": 1000000,

  "enableGovernance": true,
  "governanceTokenSymbol": "OP",
  "governanceTokenName": "Optimism",
  "governanceTokenOwner": "0x6A06226C406f7298E1F7CF3F1aa72f3Bc5eF4247",

  "l2GenesisBlockGasLimit": "0x1c9c380",
  "l2GenesisBlockBaseFeePerGas": "0x3b9aca00",

  "eip1559Denominator": 50,
  "eip1559DenominatorCanyon": 250,
  "eip1559Elasticity": 6,
  "l2GenesisFjordTimeOffset": "0x0",
  "l2GenesisRegolithTimeOffset": "0x0",
  "l2GenesisEcotoneTimeOffset": "0x0",
  "l2GenesisDeltaTimeOffset": "0x0",
  "l2GenesisCanyonTimeOffset": "0x0",
  "systemConfigStartBlock": 0,

  "requiredProtocolVersion": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "recommendedProtocolVersion": "0x0000000000000000000000000000000000000000000000000000000000000000",

  "faultGameAbsolutePrestate": "0x03c7ae758795765c6664a5d39bf63841c71ff191e9189522bad8ebff5d4eca98",
  "faultGameMaxDepth": 44,
  "faultGameClockExtension": 0,
  "faultGameMaxClockDuration": 1200,
  "faultGameGenesisBlock": 0,
  "faultGameGenesisOutputRoot": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "faultGameSplitDepth": 14,
  "faultGameWithdrawalDelay": 600,

  "preimageOracleMinProposalSize": 1800000,
  "preimageOracleChallengePeriod": 300,

  "useAltDA": true,
  "daCommitmentType": "GenericCommitment",
  "daChallengeWindow": 160,
  "daResolveWindow": 160,
  "daBondSize": 1000000,
  "daResolverRefundPercentage": 0
}

```



### User Feedback on Deployment and Configuration

> **Feedback**  
> - Update links to `alchemy-sepolia` for valid access: `https://www.alchemy.com/chain-connect/endpoints/alchemy-sepolia`  
> - Clearly specify to use `IMPL_SALT` in the `.envrc` for `contracts-bedrock`.  
> - Provide specific examples for `l2 chain` options where needed.
> - For L2 Configuration: Add necessary file setup steps for `genesis.json`, `rollup.json`, and `jwt.txt`.

---

### Directory Setup
1. **Navigate and create a new directory**  
    ```bash
    cd ~/optimism/packages/contracts-bedrock/deployments
    mkdir avail-optimism
    ```

---

### Deployment Command for Contracts
1. **Deploy contracts** (may take 15 minutes)  
    ```bash
    DEPLOYMENT_OUTFILE=deployments/artifact.json \
    DEPLOY_CONFIG_PATH=deploy-config/getting-started.json \
    forge script scripts/deploy/Deploy.s.sol:Deploy  --broadcast --private-key $GS_ADMIN_PRIVATE_KEY \
    --rpc-url $L1_RPC_URL --slow
    ```
    > **Note:** Specify `l2 chain` options for clarity.

---

### Configuring L2 Allocs
```bash
CONTRACT_ADDRESSES_PATH=deployments/artifact.json DEPLOY_CONFIG_PATH=deploy-config/getting-started.json \
STATE_DUMP_PATH=deploy-config/statedump.json forge script scripts/L2Genesis.s.sol:L2Genesis --sig 'runWithStateDump()' --chain <YOUR_L2_CHAINID>
```

> **Comment**  
> If facing `EvmError: Revert and Script failed`, users should update the `IMPL_SALT` environment variable in `.envrc`. 
> Explain: reusing the same `IMPL_SALT` will cause the second deployment to fail.

---

### L2 Configuration
After setting up the L1 layer, establish the L2 layer by generating these files:
- `genesis.json`: for the genesis block
- `rollup.json`: for rollup settings
- `jwt.txt`: for secure node communication

Navigate to the op-node directory:

```bash
cd ~/optimism/op-node
```

> **Instruction**  
> Replace `<RPC>` with your specific L1 RPC URL in the command below:

```bash
go run cmd/main.go genesis l2 \
--deploy-config ../packages/contracts-bedrock/deploy-config/getting-started.json \
--l1-deployments ../packages/contracts-bedrock/deployments/artifact.json \
--outfile.l2 genesis.json \
--outfile.rollup rollup.json \
--l1-rpc $L1_RPC_URL \
--l2-allocs ../packages/contracts-bedrock/deploy-config/statedump.json
```

Newly generated files `genesis.json` and `rollup.json` should be in the op-node package.

> **Additional Steps**  
> - Add `alt_da` configuration to `rollup.json`:
    ```json
    "alt_da": {
        "da_challenge_contract_address": "0x0000000000000000000000000000000000000000",
        "da_commitment_type": "GenericCommitment",
        "da_challenge_window": 160,
        "da_resolve_window": 160
    }
    ```

Generate the `jwt.txt` file for node communication:
```bash
openssl rand -hex 32 > jwt.txt
```

> **Reminder**  
> Move `genesis.json` and `jwt.txt` into the `op-geth` directory.

---

### Initialize and Configure op-geth

Navigate to `op-geth`:

```bash
cd ~/op-geth
```

Create a `datadir` directory, then initialize with `genesis.json`:

```bash
mkdir datadir
build/bin/geth init --datadir=datadir genesis.json
```

> **Comment**  
> Add `--state.scheme=hash` to avoid errors on next steps:
    ```bash
    ./build/bin/geth --datadir ./datadir --state.scheme=hash init genesis.json
    ```

---

### Running op-geth

To start `op-geth`:

```bash
cd ~/op-geth
./build/bin/geth \
  --datadir ./datadir \
  --http \
  --http.corsdomain="*" \
  --http.vhosts="*" \
  --http.addr=0.0.0.0 \
  --http.port=9545 \
  --http.api=web3,debug,eth,txpool,net,engine \
  --ws \
  --ws.addr=0.0.0.0 \
  --ws.port=9546 \
  --ws.origins="*" \
  --ws.api=debug,eth,txpool,net,engine \
  --syncmode=full \
  --nodiscover \
  --maxpeers=0 \
  --networkid=42069 \
  --authrpc.vhosts="*" \
  --authrpc.addr=0.0.0.0 \
  --authrpc.port=9551 \
  --authrpc.jwtsecret=./jwt.txt \
  --rollup.disabletxpoolgossip=true \
  --state.scheme=hash
```

> **Note**  
> Running `op-geth` in archive mode is recommended for full state history access.

---

### Running op-node
To run `op-node`:

> **Tip**  
> DA server URL can be retrieved from [Avail Networks Documentation](https://docs.availproject.org/docs/networks#for-devs).

```bash
cd ~/optimism/op-node
./bin/op-node \
  --l2=http://localhost:9551 \
  --l2.jwt-secret=./jwt.txt \
  --sequencer.enabled \
  --sequencer.l1-confs=5 \
  --verifier.l1-confs=4 \
  --rollup.config=./rollup.json \
  --rpc.addr=0.0.0.0 \
  --rpc.port=8547 \
  --p2p.disable \
  --rpc.enable-admin \
  --p2p.sequencer.key=$GS_SEQUENCER_PRIVATE_KEY \
  --l1=$L1_RPC_URL \
  --l1.rpckind=$L1_RPC_KIND \
  --altda.enabled=true \
  --altda.da-server=<DA_SERVER_HTTP_URL> \
  --altda.da-service=true \
  --l1.beacon.ignore=true
```

> **Reminder**  
> Mention `L1_RPC_KIND` should be set in `.envrc`.

---

### Geth Console for Genesis Block Hash

> **Feedback**  
> Start `geth` in interactive mode to retrieve starting hash:
    ```bash
    ./build/bin/geth \
      --datadir ./datadir \
      --http --http.addr=0.0.0.0 --http.port=9545 --http.api=eth,web3,net \
      --authrpc.addr=:: --authrpc.port=9551 \
      --authrpc.jwtsecret=./jwt.txt \
      console
    ```

Retrieve hash with:
```javascript
eth.getBlock(0).hash
```

---
