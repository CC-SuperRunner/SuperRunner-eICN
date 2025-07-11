# SuperRunner-eICN for Ethereum2.0

## Prerequisites

### solc

```bash
pip install solc-select
solc-select install 0.8.2
solc-select use 0.8.2
```

### protoc

```bash
# Download latest release
PROTOC_ZIP=protoc-24.3-linux-x86_64.zip  # Change version and platform accordingly
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v24.3/$PROTOC_ZIP

# Unzip and move files
# sudo apt install zip unzip -y
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'

# Verify installation
protoc --version

# install the Go plugin for Protocol Buffers
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
# For gRPC support:
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
# add $PATH
export PATH="$PATH:$(go env GOPATH)/bin"
```

### abigen

```bash
# install abigen in go-ethereum
git clone --depth 1 --branch v1.15.0 https://github.com/ethereum/go-ethereum.git
cd go-ethereum
make
make devtools
```

<!-- ## Compile contracts

```bash
# generate abi and bin for contract SR2PC
solc --abi --bin --overwrite --optimize --optimize-runs 200 -o output ../SuperRunner-Contracts/contracts/2pc-master/SR2PC.sol --allow-paths .
# generate .go
mkdir SR2PC
abigen --bin=output/SR2PC.bin --abi=output/SR2PC.abi --pkg=SR2PC --out=SR2PC/SR2PC.go
``` -->

## start geth dev mode

```bash
geth --dev --dev.period 3 --keystore ./node/keystore --allow-insecure-unlock --http --http.api eth,web3,net,miner,txpool,admin --ws --ws.api eth,web3,net --http.port 8545 --ws.port 8546
```

## start metrics collector

```bash
./metrics/run_metrics.sh
```

## deploy contract

```bash
# compile and deploy the lib Filter
# then compile and deploy the contract SR2PC
# this script strictly requires the contract and library path
./scripts/deploy.sh --config=config.yaml
```

## start eICN

```bash
go run main.go --config=config.yaml --log=logs/1.log
```

## regist eICN

```bash
./scripts/regist_eICN.sh --config=config.yaml --target-config=target_config.yaml
```

## send cross-chain message

```bash
./scripts/cross_send.sh --chain-ids="1,2,3" --value="100" --config=config.yaml
```
