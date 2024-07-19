# SP1 Telepathy

## Overview

On-chain Ethereum light client built with SP1.

- `/program`: The SP1 Telepathy program
- `/contracts`: Contracts for the Telepathy light client
- `/primitives`: Common types shared between the program, contract, and script
- `/script`: Scripts for getting the contract's genesis parameters and generating proofs


## Deployments
Test (mock proof verifier): [`sepolia:0xE9d36c391F2B4982AFc2f74C95C535d68Cdae5F4`](https://sepolia.etherscan.io/address/0xE9d36c391F2B4982AFc2f74C95C535d68Cdae5F4)

Prod: TBA when SP1 goes to mainnet

## Deploy a light client

### Prerequisites 
- [Foundry](https://book.getfoundry.sh/getting-started/installation)
- [SP1](https://docs.succinct.xyz/getting-started/install.html)
  
### 1. Generate genesis parameters

1. `cd ./script`
2. `RUST_LOG=info cargo run --release --bin genesis`

### 2. Deploy contracts

1. `cd ../contracts`
2. `cp .env.example .env`
3. Paste the genesis parameters into `.env` and manually fill in the other parameters
4. `forge install`
5. `source .env`
6. `forge script script/Deploy.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --etherscan-api-key $ETHERSCAN_API_KEY --broadcast --verify`
7. Take note of the light client contract address printed by the script
   
   ![alt text](./return-image.png)

### 3. Run light client
Continuously generate proofs & keep light client updated with chain
1. `cd ../script`
2. `cp .env.example .env`
3. Paste in the contract address in `.env` and fill out the other parameters
4. `RUST_LOG=info cargo run --release --bin operator`
