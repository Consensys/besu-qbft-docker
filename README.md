# Besu QBFT Docker Compose Setup

This repository contains a docker-compose setup for testing a Besu network with the QBFT consensus algorithm.

## Usage

To start the network, run:

```bash
docker-compose up
```
## Data persistence

The data for each node is stored in the `data` directory. This directory is mounted as a volume in the docker-compose file. This means that the data will persist between restarts.

In order to reset the data and start the network from genesis again delete the respective folders:

```bash
rm -rf ./data/validator{1,2,3,4}/*
```

## Stress Testing

To stress test the network, the [pandoras-box](https://github.com/madz-lab/pandoras-box) tool can be used. This tool will send a number of transactions to the network and will output the block utilization and tps.

### Installation

```bash
npm install -g pandoras-box
```

### Stress test with ETH transfers:

The example command will send 10000 transactions in batches of 500 tx from 100 different accounts. The output will be saved in `./myOutput.json`.

```bash
pandoras-box -url http://127.0.0.1:8545 -m "exit warm sadness vault clip rent educate pluck gentle vehicle news verb" -t 10000 -b 500 -s 100 -o ./myOutput.json
```

### Stress test with ERC20 transfers:

The example command will send 10000 ERC20 transfers in batched on 500 tx from 10 different accounts. The output will be saved in `./myOutput.json`.

```bash
pandoras-box -url http://127.0.0.1:8545 -m "exit warm sadness vault clip rent educate pluck gentle vehicle news verb" -t 10000 -b 500 -s 10 --mode ERC20 -o ./myOutput.json
```