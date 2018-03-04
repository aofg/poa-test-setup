# POA Network test setup

## How it works:
- [x] gets content of master branch of `poa-network-consensus-contracts` repo
- [x] compiles all POA Network contracts
- [x] gets binary code of POA Network Consensus contract
- [x] gets spec.json from `sokol` branch of `chain-spec` repo
- [x] generates custom private, public key and password of MoC and save them to `./keys/moc` folder
- [x] updates spec.json with new MoC and binary code of POA Network Consensus contracts
- [x] starts MoC Parity node
- [x] deploys secondary POA Network contracts
- [x] gets content of master branch of `poa-scripts-moc` repo
- [x] generates 1 initial key with password and copy it to `./keys/initial_keys` folder
- [x] gets content of sokol branch of `poa-dapps-keys-generation` repo
- [x] launches Ceremony DApp is locally builded from repo
- [x] runs e2e tests on Ceremony DApp
- [x] saves generated production keys with Ceremony DApp to `./keys` folder
- [x] runs validator node
- [x] gets content of sokol branch of `poa-dapps-validators` repo
- [x] launches Validators DApp is locally builded from repo
- [x] runs e2e tests on Validators DApp
- [x] gets content of sokol branch of `poa-dapps-voting` repo
- [x] launches Voting DApp is locally builded from repo
- [x] runs e2e tests on Governance DApp

## Requirements
1. Linux, Mac OS
2. Python 3.5+
3. [Solidity flattener](https://github.com/BlockCatIO/solidity-flattener)
4. Parity 1.9.2+

There are some options to start POA Network test setup depending on your needs:
- [Start MoC node](#start-moc-node) - launches only MoC node, generates initial key
- [Launch DApps](#launch-dapps) - launches Ceremony, Validators, Governance DApps
- [Launch Ceremony](#launch-ceremony) - conducts Ceremony
- [Set validator data](#set-validators-data) - set validators personal data with Validators DApp
- [Start MoC node + e2e Ceremony test](#start-moc-node--e2e-ceremony-test) - launches only MoC node, generates initial key, launches Ceremony Dapp and generates production keys from it
- [Start MoC + 1 validator nodes + e2e Ceremony test](#start-moc-one-validator-nodes--e2e-ceremony-test) - launches MoC node, generates initial key, launches Ceremony Dapp and generates production keys from it, launches 1 validator node, launches Validators Dapp

## Basic scenarios

### Start MoC node
1. `npm i`
2. `npm run start-test-setup`

At the successful end of POA test setup start you'll see this message `### POA test setup is configured ###`

#### Expected results:
- RPC of Parity node with unlocked MoC account will be on `http://localhost:8545`
- Spec of the network is generated to `./spec` folder
- MoC keystore file, password, private key is generated to `./keys/moc` folder
- Parity config of MoC node is generated to `./nodes/parity-moc/moc.toml` file
- Addresses of governance smart contracts are generated to `./submodules/poa-network-consensus-contracts/contracts.json`
- ABI of smart contracts are generated to `./submodules/poa-network-consensus-contracts/build/contracts`

### Launch DApps

*Note*: can be started after [previous step is completed](#start-moc-node)

1. `npm run launch-dapps`

#### Expected results:
- Ceremony Dapp is started on `http://localhost:3000`
- Validators Dapp is started on `http://localhost:3001`
- Governance Dapp is started on `http://localhost:3002`

### Launch Ceremony

*Note*: can be started after [previous step is completed](#launch-dapps)

1. `npm run launch-ceremony`

#### Expected results:
- 3 initital key are generated
- Initial key passwords, private keys are generated to `.keys/initial_keys` folder
- e2e test of Ceremony DApp is executed
- Mining addresses, passwords and private keys are copied to `./keys/mining_keys` folder
- Payout addresses, passwords and private keys are copied to `./keys/payout_keys` folder
- Voting addresses, passwords and private keys are copied to `./keys/voting_keys` folder
- Initital keys are removed
- Most ETH from initial keys are transfered to voting keys
- Validator nodes are started at RPC ports `8550`, `8551`, `8552`

### Set Validators' personal data

*Note*: can be started after [previous step is completed](#launch-ceremony)

1. `npm run set-validators-data`

#### Expected results:
- 3 validators filled with mock personal data in Validator Dapp

## Additional scenarios

### Start MoC node + e2e Ceremony test
1. `npm i`
2. `npm run start-test-setup-e2e-ceremony-test`

#### Expected results:
- All expected results from [Start MoC node script](#start-moc-node)
- e2e test of Ceremony DApp was executed
- Mining address, password and private key is copied to `./keys/mining_keys` folder
- Payout address, password and private key is copied to `./keys/payout_keys` folder
- Voting address, password and private key is copied to `./keys/voting_keys` folder

If you have already started test POA setup before with [Start MoC node script](#start-moc-node)  you can run e2e ceremony test with `npm run e2e-ceremony-test` 

### Start MoC, one validator nodes + e2e Ceremony test
1. `npm i`
2. `npm run start-moc-validator-setup`

#### Expected results:
- All expected results from [Start MoC node + e2e Ceremony test](#start-moc-node--e2e-ceremony-test)
- RPC of Parity node with unlocked validator account will be on `http://localhost:8554`
- Validators Dapp is started on `http://localhost:3001`

### Finish test POA setup
`npm run stop-test-setup`