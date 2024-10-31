# Introduction

Embark on setting up your own Avail-Optimism chain. This guide targets Ethereum's Sepolia testnet and Avail Turing testnet. For a detailed understanding, review the Optimism Documentation.

# Prerequisites

Ensure you have installed the following software.

"""
| Software | Version    |
|----------|------------|
| Git      | OS default |
| Go       | 1.21       |
| Node     | ^20        |
| Pnpm     | 8.5.6      |
| Make     | OS default |
| jq       | OS default |
| direnv   | Latest     |
| Foundry  | ^0.2.0     |
"""

> Mismatch in commands and docs

> **Note:**  
> Go version changed from `1.20` to `1.21.6`  
> Node version updated from `16` to `20`  

---

# Install Git
```bash
sudo apt install -y git curl make jq
```
> **Note:**  
> `wget` is also needed for this setup.

---

# Install Go

> **Note:**  
> Updated instructions for downloading and installing Go version `1.21.6`.

```bash
# Download and install Go 1.21.6
wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz

# Extract the downloaded tarball
tar xvzf go1.21.6.linux-amd64.tar.gz

# Move Go to the correct directory
sudo mv go /usr/local

# Set the GOROOT and update the PATH in .bashrc
echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$GOROOT/bin:$GOPATH/bin:$PATH' >> ~/.bashrc

# Reload .bashrc to apply changes
source ~/.bashrc

# Verify Go installation
go version
```

---

# Install Node.js
> **Note:**  
> Node version updated to `20`.

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


# Install Node.js
<!-- curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - -->
sudo apt-get install -y nodejs npm
 
# Install Pnpm
sudo npm install -g pnpm
 
# Install Make
sudo apt install -y make
 
# Install jq
sudo apt install -y jq
 
# Install direnv
sudo apt install -y direnv

<!-- Explicitly mention to keep this running -->
 Setup Avail DA-server
Run avail-da server ( manually ).
Navigate to
git clone https://github.com/availproject/avail-alt-da-server.git
cd avail-alt-da-server

Build Avail DA Server: make da-server

Run your DA sidecar: ./bin/avail-da-server ./cmd/avail --addr=localhost --port=8000 --avail.rpc=<WS url to an avail node> --avail.seed=<> --avail.appid=<>

<!-- WS url can be found here or can run your own DA node (harder)
https://docs.availproject.org/docs/networks
 -->

Run using docker
Copy .env.example to .env. Fill the values inside.

Run the following commands:

docker-compose build
docker-compose up

Build the Adapter Source
Clone and navigate to the Avail adapter:

git clone https://github.com/ethereum-optimism/optimism.git
cd optimism
git fetch --tag --all
git checkout v1.9.1
git submodule update --init --recursive

Install modules:

pnpm install

Compile the necessary packages:

make op-node op-batcher op-proposer
pnpm build

Build the Optimism Geth Source
Clone and navigate to op-geth:

git clone https://github.com/ethereum-optimism/op-geth.git
cd op-geth
git fetch --tag --all
git checkout v1.101408.0

Compile op-geth:

make geth



<!-- Sepolia links both invalid
Update links to 
https://www.alchemy.com/chain-connect/endpoints/alchemy-sepolia\
for alchemy which will require api key


Generate and secure keys

use of foundry not menitoned

curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc

foundryup -->

<!-- use this guide to set up avail wallet and make app id
https://docs.availproject.org/docs/end-user-guide/app-id
 -->
 <!-- go here for avail tokens for transaction fees
 https://faucet.avail.tools/
  -->
Create four essential accounts with private keys:

Admin (contract upgrade authority)
Batcher (publishes Sequencer data to L1)
Proposer (publishes L2 results to L1)
Sequencer (signs blocks on the p2p network)
You can use cast wallet in the contracts-bedrock package for key generation:

In the Optimism repo, navigate to the contracts-bedrock package:

cd ~/optimism/packages/contracts-bedrock

Generate accounts:
<!-- reordered these -->
echo "Admin:"
cast wallet new
echo "Batcher:"
cast wallet new
echo "Proposer:"
cast wallet new
echo "Sequencer:"
cast wallet new

You should see an output similar to:

Admin:
Successfully created new keypair.
Address:     0xc4A01194958DE0D90A876e8A5fc9D7B530072148
Private key: 0xb8e39bd94a210e410c4024e1cc91014de45a5eb1e42f3aa99a368b5a5ac19b45
Proposer:
Successfully created new keypair.
Address:     0xFC0374Ae658e46cA4022acA179d3cb6D8e1A4934
Private key: 0xa9bc1b3f5deb1e00251df68bf86e3493b25bc5430665433546f2f9aacc748d1a
Batcher:
Successfully created new keypair.
Address:     0xD6857B5BE9468Be67d64ABaB48459378d5329b96
Private key: 0xe9cd8960fc7984a301d567b819e0c62871eb2c7239c2e66b8f319eaa45c3cbd5
Sequencer:
Successfully created new keypair.
Address:     0x33348817E4B1192D576C4f157e9a5EC93dc5392D
Private key: 0xd98b49e11e4e0be9931017831395e6644a50c36285d08e14d1a479af5ee08675

Record and securely store these key details. You'll need to fund Admin, Proposer, and Batcher with Sepolia ETH (0.5 ETH for Admin, 0.2 ETH for Proposer, 0.1 ETH for Batcher).

NOTE FOR PRODUCTION
Use secure hardware for key management in production environments. cast wallet is not designed for production deployments.


Network Configuration and Setup
After building the repositories, configure your chain settings in the contracts-bedrock package.

Ensure you are in the contracts-bedrock sub-directory:

cd ~/optimism/packages/contracts-bedrock

Activate the environment with direnv:

If you need to install direnv, ensure you also modify the shell configuration.


<!-- .envrc file not found, instruct user to copy .envrc.example into packages/contracts-bedrock .envrc after creating it

example has types of users in wrong order, re order command to minimize mistakes

explicitly mention using saved values from cast commands in the .envrc we copied -->

direnv allow .

<!-- provide link to sample .envrc https://github.com/ethereum-optimism/optimism/blob/develop/.envrc.example -->


<!-- mention this -->
export L1_RPC_URL="https://eth-sepolia.g.alchemy.com/v2/<your-api-key>"

<!-- missed step -->
source .envrc

Core Contract Deployment
Deploy essential L1 contracts for the chain’s functionality:

Update /optimism/packages/contracts-bedrock/deploy-config and update file getting_started.json.
cd packages/contracts-bedrock
./scripts/getting-started/config.sh

<!-- mention getting_started.json is generated in deploy-config folder-->
cd deploy-config
nano getting-started.json

Add the following at the bottom of the config generated:
 "useAltDA": true,
  "daCommitmentType": "GenericCommitment",
  "daChallengeWindow": 160,
  "daResolveWindow": 160,
  "daBondSize": 1000000,
  "daResolverRefundPercentage": 0

  Example config ( for reference purpose ):

{
  "l1StartingBlockTag": "0x2c9f156ae1cc024817b920b41c102b23b5a7526e16220c517078341f9890e8bd",
  "l1ChainID": 11155111,
  "l2ChainID": 11155420,
  "l2BlockTime": 2,
  "l1BlockTime": 12,
  "maxSequencerDrift": 600,
  "sequencerWindowSize": 3600,
  "channelTimeout": 300,
  "p2pSequencerAddress": "0x15cb6e5fa6a7134F3f6f5a9130F70c1344162b77",
  "batchInboxAddress": "0xff00000000000000000000000000000000042069",
  "batchSenderAddress": "0x5bd31d51C4041108c238473f27BF5f72311b1bcF",
  "l2OutputOracleSubmissionInterval": 120,
  "l2OutputOracleStartingBlockNumber": 0,
  "l2OutputOracleStartingTimestamp": 1725740628,
  "l2OutputOracleProposer": "0x07B4c44290fD2Fba85d7924Fbf4A6499661bBe53",
  "l2OutputOracleChallenger": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
  "finalizationPeriodSeconds": 12,
  "proxyAdminOwner": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
  "baseFeeVaultRecipient": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
  "l1FeeVaultRecipient": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
  "sequencerFeeVaultRecipient": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
  "finalSystemOwner": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
  "superchainConfigGuardian": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
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
  "governanceTokenOwner": "0x79A953eAc59e40B3c7567a4cfc446c4B4912722f",
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



Navigate to /optimism/packages/contracts-bedrock/deployments, and create avail-optimism directory:
cd ~/optimism/packages/contracts-bedrock/deployments
mkdir avail-optimism

Navigate to /optimism/packages/contracts-bedrock/ and the deploy contracts (this can take up to 15 minutes):
DEPLOYMENT_OUTFILE=deployments/artifact.json \
DEPLOY_CONFIG_PATH=deploy-config/getting-started.json \
forge script scripts/deploy/Deploy.s.sol:Deploy  --broadcast --private-key $GS_ADMIN_PRIVATE_KEY \
--rpc-url $L1_RPC_URL --slow
<!-- Specify l2 chain options further with examples -->
L2 Allocs
CONTRACT_ADDRESSES_PATH=deployments/artifact.json DEPLOY_CONFIG_PATH=deploy-config/getting-started.json STATE_DUMP_PATH=deploy-config/statedump.json forge script scripts/L2Genesis.s.sol:L2Genesis --sig 'runWithStateDump()' --chain <YOUR_L2_CHAINID>

If you see a nondescript error that includes EvmError: Revert and Script failed then you likely need to change the IMPL_SALT environment variable. This variable determines the addresses of various smart contracts that are deployed via CREATE2.
<!-- specify IMPL_SALT should be part of envrc -->
If the same IMPL_SALT is used to deploy the same contracts twice, the second deployment will fail.

You can generate a new IMPL_SALT by running direnv reload anywhere in the Avail Optimism Monorepo.

Setting Up L2 Configuration
After configuring the L1 layer, focus shifts to establishing the L2 infrastructure. This involves generating three key files:

genesis.json for the genesis block
rollup.json for rollup configurations
jwt.txt for secure communication between op-node and op-geth
Navigate to the op-node directory:

cd ~/optimism/op-node

Run the following command, ensuring you replace <RPC> with your specific L1 RPC URL. This generates the genesis.json and rollup.json files:

go run cmd/main.go genesis l2 \
--deploy-config ../packages/contracts-bedrock/deploy-config/getting-started.json \
--l1-deployments ../packages/contracts-bedrock/deployments/artifact.json \
--outfile.l2 genesis.json \
--outfile.rollup rollup.json \
--l1-rpc $L1_RPC_URL \
--l2-allocs ../packages/contracts-bedrock/deploy-config/statedump.json

You'll find the newly created genesis.json and rollup.json in the op-node package.

Add the following at the end of rollup.json:

 "alt_da": {
    "da_challenge_contract_address": "0x0000000000000000000000000000000000000000",
    "da_commitment_type": "GenericCommitment",
    "da_challenge_window": 160,
    "da_resolve_window": 160
  }

Generate a jwt.txt file, which is crucial for the secure interaction between nodes:

openssl rand -hex 32 > jwt.txt

To get op-geth ready, move the genesis.json and jwt.txt files into its directory:

cp genesis.json ~/op-geth
cp jwt.txt ~/op-geth

These steps ensure the L2 layer is correctly configured and ready for integration with the L1 components, paving the way for a fully functional EVM Rollup on the Avail-OP Stack.

Initialize and Configure Geth
Prepare op-geth for running the chain:

Navigate to op-geth:

cd ~/op-geth

Create a data directory:

mkdir datadir

Initialize with the genesis file:

build/bin/geth init --datadir=datadir genesis.json
<!-- 
 ./build/bin/geth --datadir ./datadir --state.scheme=hash init genesis.json 
i had to specify state.scheme to avoid error in next step
-->
Running op-geth
To initiate op-geth, navigate to its directory and execute the following commands:

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

op-geth is now active, but block creation will begin once op-node is operational.

Why Archive Mode?
Archive mode, requiring more disk space than full mode, is essential for:

op-proposer to access the full state history.
The explorer's functionality.
Reinitializing op-geth
In cases of database corruption indicated by op-node errors or failure to find L2 heads, follow these steps:

Stop op-geth.
Remove the existing data:
cd ~/op-geth
rm -rf datadir/geth

Reinitialize:
build/bin/geth init --datadir=datadir genesis.json

Restart op-geth and then op-node.

<!-- specify L1_RPC_KIND has to be set in envrc-->

Running op-node
To launch op-node, which acts as a consensus client, run:
<!-- DA server can be taken from https://docs.availproject.org/docs/networks#for-devs 
    It is the same as rpc url
 -->
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

Block creation will commence once op-node starts processing L1 information and interfaces with op-geth.

P2P Synchronization
To optimize synchronization and avoid network resource waste:

Disable p2p sync (--p2p.disable) by default.
Use specific command line parameters for synchronization among multiple nodes.

<!-- I had to run geth with console on with this command to obtain starting hash and put it in op-node/genesis.json l2 hash manually

1. Start Geth in Attach Mode
To start Geth and allow for interactive commands, use:

bash

./build/bin/geth \
  --datadir ./datadir \
  --http --http.addr=0.0.0.0 --http.port=9545 --http.api=eth,web3,net \
  --authrpc.addr=:: --authrpc.port=9551 \
  --authrpc.jwtsecret=./jwt.txt \
  console
This command will start Geth with an interactive console where you can run JavaScript commands.

2. Retrieve the Genesis Block Hash
Once you have the console open, retrieve the hash of the genesis block by running:


eth.getBlock(0).hash

 -->

 Block creation will commence once op-node starts processing L1 information and interfaces with op-geth.

P2P Synchronization
To optimize synchronization and avoid network resource waste:

Disable p2p sync (--p2p.disable) by default.
Use specific command line parameters for synchronization among multiple nodes.
Running op-batcher
op-batcher is crucial in publishing transactions from the Sequencer to L1. Ensure it has at least 1 Sepolia ETH for operational continuity.

cd ~/optimism/op-batcher
./bin/op-batcher \
  --l2-eth-rpc=http://localhost:9545 \
  --rollup-rpc=http://localhost:8547 \
  --poll-interval=1s \
  --sub-safety-margin=6 \
  --num-confirmations=1 \
  --safe-abort-nonce-too-low-count=3 \
  --resubmission-timeout=30s \
  --rpc.addr=0.0.0.0 \
  --rpc.port=8548 \
  --rpc.enable-admin \
  --max-channel-duration=1 \
  --l1-eth-rpc=$L1_RPC_URL \
  --private-key=$GS_BATCHER_PRIVATE_KEY \
  --altda.enabled=true \
  --altda.da-service=true \
  --altda.da-server=<DA_SERVER_HTTP_URL>

Controlling Batcher Costs
Adjust the --max-channel-duration=n setting to balance transaction frequency on L1 and the operational costs of the batcher. Recommended is a minumum of 2 since avail block time is 20s and ethereum's 12sec.

Running op-proposer
Finally, start op-proposer to propose new state roots:

cd ~/optimism/op-proposer
./bin/op-proposer \
  --poll-interval=12s \
  --rpc.port=9560 \
  --rollup-rpc=http://localhost:8547 \
  --l2oo-address=$L2OO_ADDR \
  --private-key=$PROPOSER_KEY \
  --l1-eth-rpc=$L1_RPC
  
  <!-- inconsistent with docs, it should be   --l1-eth-rpc=$L1_RPC_URL -->
  <!-- Missing args in docs
  lvl=crit msg="Application failed" message="failed to setup: invalid CLI flags: neither the `DisputeGameFactory` nor `L2OutputOracle` address was provided"
   -->

   <!-- Adding this works
    --game-factory-address=0x0000000000000000000000000000000000000000 --proposal-interval 3s

 -->

Acquire Sepolia ETH for Layer 2
To obtain ETH on your Rollup:

Go to contracts-bedrock:

cd ~/optimism/packages/contracts-bedrock

Find the L1 standard bridge contract address:

cat deployments/avail-optimism/L1StandardBridgeProxy.json | jq -r .address

Send Sepolia ETH to the bridge contract address.

Conduct Test Transactions
You now have a fully operational Avail-Powered Optimism-based EVM Rollup. Experiment with it as you would with any other test blockchain.

Congratulations on setting up your chain!

<!-- bottom of page link op stack button for github doesnt work -->