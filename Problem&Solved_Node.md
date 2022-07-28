# Disini saya merangkum beberapa masalah dan cara penyelesaianya silahkan merujuk ke dokumentasi ini jika ada error saat melakukan instalasi node validator

## Jika anda mendapat error saat menjalankan node awal seperti contoh dibawah ini :
<img width="1460" alt="Jepretan Layar 2022-07-27 pukul 01 15 53" src="https://user-images.githubusercontent.com/55140596/181187422-f3960ae7-ba3f-4296-af21-67f36179e63c.png">

Lakukan kill pada proses pid user neard itu akan mengehentikan semua layanan dan kamu bisa memulai lagi node anda.

Cek proses yang berjalan.
````
top
````

<img width="638" alt="Jepretan Layar 2022-07-27 pukul 01 16 06" src="https://user-images.githubusercontent.com/55140596/181187481-989e8f61-885d-44bf-a1f2-8f21e50f4d91.png">

kill proses pid neard

contoh :
````
kill 41570
````
* *ganti_dengan_nomer_pid kalian*

setelah itu anda bisa kembali memulai menjalankan node anda.


# Untuk "node not producing" 😢😢

coba ini  :
````
near view <POOL_ID> get_staking_key '{}'
````
````
cat ~/.near/validator_key.json | grep public_key
````

## 🗒️  ️ Catatan: Kedua kunci harus cocok. Jika mereka TIDAK memperbarui kunci staking pool ❗ 

Update staking pool key:
````
near call <POOL_ID> update_staking_key '{"stake_public_key": "<PUBLIC_KEY>"}' --accountId <ACCOUNT_ID>\
````

Jika cocok:

Periksa validator_key.json

account_id harus seperti : xx.factory.shardnet.near

If not update it, save and stop/start neard 😉

## A hardfork is announced, what should I do? (For shardnet on July 2022)

On July 27th a third hardfork was done during stake wars to Shardnet. This for upgrading core code and keep nodes with higher stability.

Run the following to upgrade:

````
sudo systemctl stop neard
rm ~/.near/data/*

cd ~/nearcore
git fetch
git checkout 0d7f272afabc00f4a076b1c89a70ffc62466efe9
cargo build -p neard --release --features shardnet

cd ~/.near
rm genesis.json
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json

rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json

sudo systemctl start neard && journalctl -n 100 -f -u neard | ccze -A
````

Also, if you owner account or staking pool doesn't appear is probably that it was removed during hard fork. Make those again.

