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
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "cenbacen", "owner_id": "cenbacen.shardnet.near", "stake_public_key": "ed25519:CiGre44rssxxxxxxxxxx", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="cenbnacen.shardnet.near" --amount=30 --gas=300000000000000
````
#### *EDIT* cenbacen menjadi nama wallet kalian

#### *EDIT* ed25519:CiGre44rssxxxxxxxxxx dengan public_key kalian yang berada di file validator_key.json

* **Catatan amount:30 adalah minimum penyimapanan yang diperlukan jangan merubahnya lebih sedikit**

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
````
near call cenbacen.factory.shardnet.near deposit_and_stake --amount xx --accountId cenbacen.shardnet.near --gas=300000000000000
````
* **Ganti id pool/nama wallet Anda (cenbacen), id akun (cenbacen.shardnet.near) dengan ID Anda sendiri/nama wallet.**

* **Ganti amount xx dengan seat price yang bisa anda lihat disini harga seat price https://explorer.shardnet.near.org/nodes/validators**

* **Jadi misal set price 200 Near ubah menjadi amount menjadi 200 atau lebih agar validator dapat bergabung dengan saat epoch dan block sudah berganti**

## selanjutnya lakukan ping awal

````
near call cenbacen.factory.shardnet.near ping '{}' --accountId cenbacen.shardnet.near --gas=300000000000000
````
* **Ganti cenbacen dengan nama wallet kamu**

## Ping untuk mengeluarkan proposal baru dan memperbarui saldo staking untuk delegator Anda. Ping harus dikeluarkan setiap epoch untuk menjaga agar hadiah yang dilaporkan tetap terkini.

## pasang Script Auto Ping
## Script Auto Ping

* **Jalankan Script dibawah ini**

````
touch ping.sh
````

* **Ubah File Menggunakan Nano**

````
nano ping.sh
````

* **Masukkan Data ini**
````
export LOGS=/root/logs
export POOLID=cenbacen
export ACCOUNTID=cenbacen
root@localhost:~# echo "---" >> $LOGS/all.log
date >> $LOGS/all.log
near call $POOLID.factory.shardnet.near ping '{}' --accountId $ACCOUNTID.shardnet.near --gas=300000000000000 >> $LOGS/all.log
````
* **ubah cenbacen menjadi nama wallet kamu**

* **Lanjut Jalankan Ini**

````
chmod 755 ping.sh
````

* **Install Cron Job**
````
apt-get install cron
````

* **edit variabel**
````
crontab -e
````
* **saya mengunakan vps digital ocean ikuti saja sesuai perintah**

* **paste ini ke paling bawah**
````
NEAR_ENV=shardnet
*/5 * * * * sh /root/ping.sh
````

* **Done**

* **Note jika anda mengunakan vps lain coba sesuaikan dengan nama direktori anda**

* **example:**
````
*/5 * * * * sh /home/<USER_ID>/ping.sh
````
* **Buat Folder Log untuk melihat riwayat transaksi**

* **Kembali ke Folder Awal**
````
mkdir /root/logs 
````
* **Lihat Log**

* **Masuk ke folder Log**
````
nano all.log
````

## Next challenge lets fucking gooo baby

[Check your Node.](./Halaman_4.md).





