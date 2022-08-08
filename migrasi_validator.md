# Migrasikan validator Anda ke Virtual Machine Baru.

### 1. Sebelum migrasi ke mesin baru backup dahulu file node_key.json dan validator_key.json anda.
````
cd .near
````
````
cat node_key.json
````
````
cat validator_key.json 
````
* *Simpan semua kedalam notepad anda*

Pastikan pula anda memiliki akses ke wallet shardnet anda.

# Saatnya menjalankan full node di mesin baru.

### 2. Jalankan full node ke virtual machine baru

Silahkan mengikuti panduan ini sampai halaman 2 [Setup and Run your Node](./Halaman_1.md)

Check node baru sampai status false
````
apt install jq
````
````
curl -s http://127.0.0.1:3030/status | jq .sync_info
````
status harus : "false"

### 3. Tunggu hingga Node penuh di mesin baru selesai dan selesai mendownload HEADERS DAN BLOCK.

Sebelum mematikan node dan validator di mesin lama cek ketinggian block pada node lama dan node baru.

Pastikan ketinggian block pada mesin baru lebih tinggi dari pada mesin lama.

Anda bisa mematikan validator lama dan menonaktifkan system di validator lama.
````
sudo systemctl stop neard
````
````
sudo systemctl disable neard
````
### 4. Salin semua file node_key.json dan validator_key.json di mesin baru.
````
cd .near
````
````
nano node_key.json
````
* *Salin Node_key.json di validator lama ke validator baru*

Check Validator_key.json di mesin baru jika belum ada filenya anda bisa membuatnya disini.
````
near generate-key <pool_id>
````
<pool_id> ---> anda bisa membuatnya sama dengan nama yang lama.

Copy the file generated ke shardnet folder:
````
cp ~/.near-credentials/shardnet/YOUR_WALLET.json ~/.near/validator_key.json
````
YOUR_WALLET ganti dengan nama yang anda buat sebelumnya.

Masuk ke folder validator_key.json
````
cd .near
````
````
nano validator_key.json
````
* *Masukkan Private_key.json lama ke Validator yang baru.*

### 5. Login wallet Shardnet Near kamu.
````
near login
````
* *Login seperti biasanya*

### 6. Edit systemd dan Jalankan Node.

````
sudo nano /etc/systemd/system/neard.service
````
Paste:
Jika anda mengunakan vps digital ocean silahkan menggunakan ini atau anda menjalankan di mode root.
````
[Unit]
Description=NEARd Daemon Service

[Service]
Type=simple
User=root
#Group=near
WorkingDirectory=/root/.near
ExecStart=/root/nearcore/target/release/neard run
Restart=on-failure
RestartSec=30
KillSignal=SIGINT
TimeoutStopSec=45
KillMode=mixed

[Install]
WantedBy=multi-user.target
````
Dibawah ini adalah path asli yang bisa anda rubah menyesuaikan dengan direktori vps yang kalian gunakan

Perhatikan di bagian 

WorkingDirectory

dan

ExecStart
````
[Unit]
Description=NEARd Daemon Service
[Service]
Type=simple
User=<USER>
#Group=near
WorkingDirectory=/home/<USER>/.near
ExecStart=/home/<USER>/nearcore/target/release/neard run
Restart=on-failure
RestartSec=30
KillSignal=SIGINT
TimeoutStopSec=45
KillMode=mixed
[Install]
WantedBy=multi-user.target
````
> Note: Ubah USER dengan paths kamu

Langkah Selanjutnya aktifkan systemctl
Command:
````
sudo systemctl enable neard
````
Command:
````
sudo systemctl start neard
````
Jika Anda perlu melakukan perubahan pada layanan karena kesalahan dalam file. Itu harus dimuat ulang:
````
sudo systemctl reload neard
````

Jika Terdapat Log

* **No Journal files were found**

Lakukan Perintah Ini
````
sudo nano /etc/systemd/journald.conf
````
Ubah Config
````
[Journal]
#Storage=auto
...
````
Menjadi 
````
[Journal]
Storage=persistent
...
````
Dan lakukan Restart 
````
sudo systemctl restart systemd-journald
````
Cek Log Lagi Jika Masih Error Coba Restar Lagi Dan Check Log Lagi

`Jika log masih error coba restar vps atau matikan hidupkan lagi vps `

### 7. Lakukan Ping dan pasang Sciprt Auto Ping.
````
near call cenbacen.factory.shardnet.near ping '{}' --accountId cenbacen.shardnet.near --gas=300000000000000
````
* **Ganti cenbacen dengan nama wallet kamu**

## Ping untuk mengeluarkan proposal baru dan memperbarui saldo staking untuk delegator Anda. Ping harus dikeluarkan setiap epoch untuk menjaga agar hadiah yang dilaporkan tetap terkini. untuk menghindari agar anda tidak dikeluarkan dari validator.

## Pasang Script Auto Ping Ada di Halaman 6 untuk challenge 6
[Auto Ping.](./Halaman_6.md)

Pastikan Node baru mendapatkan Block dan masuk di List Validator sebagai proposal yang selanjutnya akan menjadi validator aktif.

* *Setelah itu anda dapat mematikan server di validator lama*
