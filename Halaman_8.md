# Stake Wars: Episode III. Challenge 008

* Rewards: 50 Unlocked NEAR Points (UNP) + 30 Delegated NEAR Points (DNP)

Apakah Anda ingin berkontribusi pada proyek lain yang menjalankan validator? Terapkan kontrak cerdas pada akun pemilik dari staking pool.

Kontrak ini akan memungkinkan pengguna untuk membagi hadiah mereka di beberapa akun.

## Steps

Install cargo and Rust in case you don't have it. This command is for Linux or MacOS.

```
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env
```

Add the wasm32-unknown-unknown toolchain

```
rustup target add wasm32-unknown-unknown
```

Clone the project found [here](https://github.com/zavodil/near-staking-pool-owner)

```
git clone https://github.com/zavodil/near-staking-pool-owner
```
Compile smart contract

```
cd near-staking-pool-owner/contract
cargo build --target wasm32-unknown-unknown --release
```

Deploy smart contract on your owner account. Adjust the path to .wasm file if required.
```
NEAR_ENV=shardnet near deploy <OWNER_ID>.shardnet.near --wasmFile target/wasm32-unknown-unknown/release/contract.wasm
```

Initialize the smart contract picking accounts for splitting revenue.
```
CONTRACT_ID=<OWNER_ID>.shardnet.near

NEAR_ENV=shardnet near call $CONTRACT_ID new '{"staking_pool_account_id": "<STAKINGPOOL_ID>.factory.shardnet.near", "owner_id":"<OWNER_ID>.shardnet.near", "reward_receivers": [["<SPLITED_ACCOUNT_ID_1>.shardnet.near", {"numerator": 3, "denominator":10}], ["<SPLITED_ACCOUNT_ID_2>.shardnet.near", {"numerator": 70, "denominator":100}]]}' --accountId $CONTRACT_ID
```
* *Split account id pertama bisa anda beri nama saya "cenbacen" dan split_account_id_2 bisa beri nama validator anda.

Tunggu beberapa jam hingga dan anda siap untuk menarik hadiah.

```
CONTRACT_ID=<OWNER_ID>.shardnet.near
NEAR_ENV=shardnet near call $CONTRACT_ID withdraw '{}' --accountId $CONTRACT_ID --gas 200000000000000
```

## Kriteria penerimaan:

* Penarikan berhasil didistribusikan ke 2 akun.
* Jika bug ditemukan, umpan balik terperinci dapat diambil sebagai alternatif.

## Pengajuan tantangan

* URL Tantangan: Tautan ke penjelajah transaksi *penarikan* Anda.
* Gambar tantangan: Tangkapan layar transaksi distribusi token.

![img](./images/split-log.png)

[Submit the form](https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform) with your distribution transactions.

## Penafian

Ini adalah kode yang diberikan oleh komunitas, tidak ada pengetahuan tentang audit. Lakukan tinjauan kode Anda sendiri untuk memastikan keamanan. Gunakan dengan risiko Anda sendiri.
