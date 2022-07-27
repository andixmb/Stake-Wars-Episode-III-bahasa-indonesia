# Stake Wars: Episode III. Challenge 006
## Script Auto Ping
Buat tugas cron pada mesin yang menjalankan validator node yang memungkinkan ping ke jaringan secara otomatis.
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
#!/bin/sh
# Ping call to renew Proposal added to crontab

export NEAR_ENV=shardnet
export LOGS=/root/logs
export POOLID=cenbacen
export ACCOUNTID=cenbacen

echo "---" >> $LOGS/all.log
date >> $LOGS/all.log
near call $POOLID.factory.shardnet.near ping '{}' --accountId $ACCOUNTID.shardnet.near --gas=300000000000000 >> $LOGS/all.log
near proposals | grep $POOLID >> $LOGS/all.log
near validators current | grep $POOLID >> $LOGS/all.log
near validators next | grep $POOLID >> $LOGS/all.log
````
* **Ubah cenbacen menjadi nama wallet kamu**

* **Lanjut Jalankan Ini**

````
chmod 755 ping.sh
````

* **Install Cron Job**
````
apt-get install cron
````

* **Edit variabel**
````
crontab -e
````
## Saya mengunakan vps digital ocean ikuti saja sesuai perintah**

* **Paste ini ke paling bawah**
````
NEAR_ENV=shardnet
*/5 * * * * sh /root/ping.sh
````

## Done

* **Note jika anda mengunakan vps lain coba sesuaikan dengan urutan direktori anda**

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
## Pengajuan tantangan

* URL Tantangan: Tautan ke penjelajah kumpulan taruhan Anda.
* Gambar tantangan: Tangkapan layar transaksi PING yang dilakukan secara berkala.

<img width="381" alt="Jepretan Layar 2022-07-27 pukul 13 55 38" src="https://user-images.githubusercontent.com/55140596/181184527-091a2c6e-6b13-4cb8-b1fe-000ad5bfc590.png">

[Kirim formulir](https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform) dengan transaksi PING Anda.

## Opsional: Gunakan croncat untuk membuat tugas ping
