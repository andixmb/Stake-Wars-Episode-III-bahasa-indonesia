# Stake Wars: Episode III. Challenge 003

## Terapkan kumpulan staking baru untuk validator Anda. Lakukan operasi pada staking pool Anda untuk mendelegasikan dan stake DEKAT.

### Sebelum memulai pastikan node anda sudah men-download block 100% jika anda masih melihat logs ( block downloads ) jangan pernah mencoba membuat kumpulan dan mendelegasikan NEAR. Anda akan menyerang jaringan dan jika semua orang melakukan hal itu jaringan akan DOWN tolong untuk tetap mengikuti peraturan.

#### Check Node Block
````
journalctl -n 100 -f -u neard | grep "INFO stats"
````

#### Log Block masih di download

<img width="831" alt="Jepretan Layar 2022-07-22 pukul 02 31 11" src="https://user-images.githubusercontent.com/55140596/180300131-188e5afb-dcec-4b66-ab53-388e80dac8b4.png">

#### Lanjutkan Deploy a Staking Pool jika anda sudah tidak melihat Downloading blocks

````
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "cenbacen", "owner_id": "cenbacen.shardnet.near", "stake_public_key": "ed25519:CiGre44rssxxxxxxxxxx", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="gateomega.shardnet.near" --amount=30 --gas=300000000000000
````
#### *EDIT* cenbacen menjadi nama wallet kalian

#### *EDIT* ed25519:CiGre44rssxxxxxxxxxx dengan public_key kalian yang berada di file validator_key.json

Anda akan melihat LOG  "True" di Akhir LOG Create Pool yang Anda buat.

## Penjelasan Singkat Mengenai Contoh Diatas:

* **Pool ID**: Staking pool name, progam secara otomatis menambahkan namanya ke parameter ini, membuat {pool_id}.{staking_pool_factory} Contoh: cenbacen.factory.shardnet.near

Di dalam direktory .near-credentials/shardnet#

* **Owner ID**: Akun SHARDNET yang akan mengelola kumpulan staking.
* **Public Key**: Public Key dalam file validator_key.json Anda.
* **5**: Biaya yang akan dikenakan oleh pool (misalnya dalam hal ini 5 di atas 100 adalah 5% dari biaya).
* **IAccount Id**: Akun SHARDNET yang menerapkan kumpulan staking.

Periksa pool Anda sekarang yang akan terlihat di sini https://explorer.shardnet.near.org/nodes/validators

## Deposit and Stake NEAR

* **near call cenbacen.factory.shardnet.near deposit_and_stake --amount xx --accountId cenbacen.shardnet.near --gas=300000000000000**

* **Ganti id pool/nama wallet Anda (cenbacen), id akun (cenbacen.shardnet.near) dengan ID Anda sendiri/nama wallet.**

* **Ganti amount dengan seat price yang bisa anda lihat disini harga seat price https://explorer.shardnet.near.org/nodes/validators**

## Ping

Ping mengeluarkan proposal baru dan memperbarui saldo staking untuk delegator Anda. Ping harus dikeluarkan setiap epoch untuk menjaga agar hadiah yang dilaporkan tetap terkini.

near call <staking_pool_id> ping '{}' --accountId <accountId> --gas=300000000000000

  Auto-Ping Script
  
  Anda harus melakukan ping ke server setiap 5 menit untuk mengaktualisasikan hadiah Anda dan menjaga node Anda tetap aktif jika ditendang keluar.
  
  Buat corn job
  ````
  apt-get install cron
  ````
  near_ping.sh
  ````
  cat near_ping.sh
  ````
  


