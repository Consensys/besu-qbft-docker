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

The example command will send 10000 transactions in batches of 500 tx from 100 different accounts. The output will be saved in `./myOutput.json`. The address associated with the mnemonic used in the example has already been funded with ETH during genesis.

```bash
pandoras-box -url http://127.0.0.1:8545 -m "exit warm sadness vault clip rent educate pluck gentle vehicle news verb" -t 10000 -b 500 -s 100 -o ./myOutput.json
```

### Stress test with ERC20 transfers:

The example command will send 10000 ERC20 transfers in batched on 500 tx from 10 different accounts. The output will be saved in `./myOutput.json`.

```bash
pandoras-box -url http://127.0.0.1:8545 -m "exit warm sadness vault clip rent educate pluck gentle vehicle news verb" -t 10000 -b 500 -s 10 --mode ERC20 -o ./myOutput.json
```

## Local docker image

Instead of using official Besu docker images, a local docker image can be built and used. This is useful for testing changes to Besu.

In the Besu directory, run:

```bash
./gradlew -Prelease.releaseVersion=24.3.3-local distDocker # 24.3.3-local is used as an example version
```

It is important to use a non-existing version number to avoid conflicts with official Besu images. Appending `-local` to the version number is a common practice.

After building the image, update the `docker-compose.yml` file to use the local image or one or more validators:

Replace
```yaml
image: hyperledger/besu:latest
```
with 
```yaml
image: hyperledger/besu:24.3.3-local
```

Then, run `docker-compose up` to start the network with the local image.