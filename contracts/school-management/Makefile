-include .env

default: build

all: test

test: build
	cargo test

build:
	stellar contract build
	@ls -l target/wasm32v1-none/release/*.wasm

fmt:
	cargo fmt --all

clean:
	cargo clean

upload: build
	@echo "📦 Uploading contract..."
	@stellar contract install \
	  --network ${TESTNET} \
	  --source ${SOURCE_TESTNET} \
	  --wasm ../../target/wasm32v1-none/release/hello_world.wasm > upload.log 2>&1 || (cat upload.log && exit 1)
	@grep -oE "[a-f0-9]{64}" upload.log | tail -n 1 > wasm_hash.txt
	@echo "✅ Wasm hash saved to wasm_hash.txt"
	@cat wasm_hash.txt

deploy:
	stellar contract deploy \
	  --wasm-hash ${WASM_HASH} \
	  --network ${TESTNET} \
	  --source-account ${SOURCE_TESTNET} \
	  -- \
	  --usdc_asset ${USDC_TESTNET_ASSET} \
	  --settings_contract ${LM_CONTRACT_ADDRESS}

interact:
	stellar contract invoke \
		--id ${CONTRACT_ADDRESS} \
		--source-account ${SOURCE_TESTNET} \
		--network ${TESTNET} \
		--fee ${FEE} \
		-- \
		get_token_balance \
		--user ${ADMIN_TESTNET}