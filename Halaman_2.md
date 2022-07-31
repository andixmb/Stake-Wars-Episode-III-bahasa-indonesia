# Stake Wars: Episode III. Challenge 002

* Submitted by: Open Shards Alliance
* Rewards: 30 Unlocked NEAR Points (UNP)

## Tantangan ini difokuskan pada penerapan node (nearcore), mengunduh snapshot, menyinkronkannya ke keadaan jaringan yang sebenarnya, kemudian mengaktifkan node sebagai validator.

##### Prerequisites:
Sebelum memulai, Anda mungkin ingin memastikan bahwa mesin Anda memiliki fitur CPU yang tepat.

```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
```
Hasil

> Supported


##### Install developer tools:
```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python2-minimal docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```
#####  Install Python pip:

```
sudo apt install python3-pip
```
##### Set the configuration:

```
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```

##### Install Building env
```
sudo apt install clang build-essential make
```

##### Install Rust & Cargo
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Anda akan melihat yang berikut ini:

![rust](https://user-images.githubusercontent.com/55140596/180235181-db2fbc43-a4b7-49f8-ac6c-9fc2eb90af4f.png)


Pilih 1 dan tekan enter.

##### Source the environment
```
source $HOME/.cargo/env
```

#### Clone `nearcore` project from GitHub
Pertama, clone the [`nearcore` repository](https://github.com/near/nearcore).

````
git clone https://github.com/near/nearcore
````
````
cd nearcore
````
````
git fetch
````
#### Checkout ke komit yang diperlukan
````
git checkout c1b047b8187accbf6bd16539feb7bb60185bdc38
````
#### Compile `nearcore` binary

Di folder `nearcore` jalankan perintah berikut:

```
cargo build -p neard --release --features shardnet
```
Jalur biner adalah `target/release/neard`. Jika Anda melihat masalah, mungkin perintah kargo tidak ditemukan. Kompilasi biner `nearcore` mungkin memakan waktu agak lama.

#### Initialize working directory

Agar berfungsi dengan baik, node NEAR memerlukan direktori kerja dan beberapa file konfigurasi. Hasilkan direktori kerja awal yang diperlukan dengan menjalankan:

```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```

![initialize](https://user-images.githubusercontent.com/55140596/180235376-7f32d0d3-d73a-4130-9601-d907ba42c7e8.png)

Hanya penjelasan
Perintah ini akan membuat struktur direktori dan akan menghasilkan `config.json`, `node_key.json`, dan `genesis.json` pada jaringan yang telah Anda lewati.

- `config.json` - Parameter konfigurasi yang responsif terhadap cara kerja node. config.json berisi informasi yang diperlukan agar node dapat berjalan di jaringan, cara berkomunikasi dengan peer, dan cara mencapai konsensus. Meskipun beberapa opsi dapat dikonfigurasi. Secara umum validator telah memilih untuk menggunakan config.json default yang disediakan.

- `genesis.json` - File dengan semua data yang dimulai jaringan di genesis. Ini berisi akun awal, kontrak, kunci akses, dan catatan lain yang mewakili keadaan awal blockchain. File genesis.json adalah snapshot dari status jaringan pada suatu titik waktu. Di akun kontak, saldo, validator aktif, dan informasi lain tentang jaringan.

- `node_key.json` - File yang berisi kunci publik dan pribadi untuk node. Juga menyertakan parameter `account_id` opsional yang diperlukan untuk menjalankan node validator (tidak tercakup dalam dokumen ini).

- `data/` - Folder tempat node NEAR akan menulis statusnya.

#### Replace the `config.json`
Kembali ke folder awal
````
cd ..
````
next step

```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```
#### Check status 
Pada folder config.json 

Jika isi file `"tracked_shards": []` ( tambahkan angka 0) `"tracked_shards": [0]`

Dan dibagian archive": false, jika masih true edit menjadi false

Edit mengunakan nano

File config.json berada di folder .near

````
cd .near
````
````
nano config.json
````
Cari `"tracked_shards" dan archive":

Edit dan save 

ctrl + x lalu y lalu enter

Install AWS Cli
```
sudo apt-get install awscli -y
```
Jika hal di atas gagal, AWS CLI mungkin mengalami oudated di repositori distribusi Anda. Sebagai gantinya, coba:
```
pip3 install awscli --upgrade
```

#### Run the node
Untuk memulai node Anda cukup jalankan perintah berikut:

Jalankan di folder nearcore

````
cd nearcore
````
````
./target/release/neard --home ~/.near run
````
# Jika terminal anda terputus silahkan merujuk ke dokumentasi ini agar anda bisa melihat log nya kembali [Problem&Solved](./Problem&Solved_Node.md)
<img width="1510" alt="download" src="https://user-images.githubusercontent.com/55140596/180240793-807762d1-d863-494f-b7d3-3d7078774f8e.png">

Node sedang berjalan, Anda dapat melihat output log di konsol Anda. Node Anda harus menemukan rekan, mengunduh HEADERS hingga 100%, dan kemudian mengunduh blok.
----
* **LIHAT HEADERS DOWNLOAD**

<img width="358" alt="Jepretan Layar 2022-07-21 pukul 16 55 31" src="https://user-images.githubusercontent.com/55140596/180241246-3a143ec2-59c1-40db-b3d1-3863ed606172.png">

* **LOG JIKA HEADER SUDAH TERINSTAL DAN MELANJUTKAN DOWNLOAD BLOCK**

![0476ec55-b827-4365-8258-05d97edb3d98](https://user-images.githubusercontent.com/55140596/180241813-af36e65d-a06b-4871-8035-5d2ad85419ba.jpeg)

* **LOG DOWNLOAD BLOCK SELESAI 100%**

<img width="618" alt="Jepretan Layar 2022-07-21 pukul 16 55 12" src="https://user-images.githubusercontent.com/55140596/180242032-2d4e8a47-d95b-4da4-b4c8-62b9aa949697.png">

* **SETELAH SEMUANYA SIAP KELUAR DARI LOG**
````
ctrl + c
````

### Mengaktifkan node sebagai validator
##### Otorisasi Dompet Secara Lokal
Kunci akses penuh perlu diinstal secara lokal untuk dapat menandatangani transaksi melalui NEAR-CLI.

* You need to run this command:

```
near login
```

> Catatan: Perintah ini meluncurkan browser web yang memungkinkan otorisasi kunci akses penuh untuk disalin secara lokal.

1 – Salin tautan ke browser Anda


![1](https://user-images.githubusercontent.com/55140596/180243116-711d5c35-eaec-4dfc-81f6-c87225d4ecfa.png)

2 – Grant Access to Near CLI

![3](https://user-images.githubusercontent.com/55140596/180243146-f67ae837-4b06-4917-ab6d-c6d4e4ae5aad.png)

3 – Setelah Grant, kamu akan melihat halaman browser error, kembali ke console

![4](https://user-images.githubusercontent.com/55140596/180243152-396aaa63-f166-4984-a0d1-94dc56173db5.png)

4 – Paste nama wallet ke console dan enter kamu akan melihat sukses menautkan wallet dengan console anda

![5](https://user-images.githubusercontent.com/55140596/180243166-6336f82b-bf49-4a87-8799-fd0df621da12.png)

catatan :
Jika terjadi error saat login wallet jangan paste wallet ke terminal dahulu lihat di browsure kalian sampai menunjukkan seperti gambar no.3 baru login ke wallet kalian di terminal


#####  Check the validator_key.json
* Jalankan perintah berikut:
```
cat ~/.near/validator_key.json
```


> Catatan: Jika validator_key.json tidak ada, ikuti langkah-langkah ini untuk membuatnya

Create a `validator_key.json` 

*   Generate the Key file:

```
near generate-key <pool_id>
```
<pool_id> ---> xx.factory.shardnet.near dimana xx nama wallet kamu

* Copy the file generated ke shardnet folder:

```
cp ~/.near-credentials/shardnet/YOUR_WALLET.json ~/.near/validator_key.json
```
YOUR_WALLET ganti dengan nama wallet kamu

Masuk ke folder validator_key.json

````
cd .near
````
````
nano validator_key.json
````

* Edit “account_id” => xx.factory.shardnet.near, dimana xx nama wallet kamu tadi
* Ganti `private_key` to `secret_key`

>Catatan: ID_akun harus cocok dengan nama kontrak kumpulan taruhan atau Anda tidak akan dapat menandatangani blok.\

Konten file harus dalam pola berikut:
```
{
  "account_id": "xx.factory.shardnet.near",
  "public_key": "ed25519:HeaBJ3xLgvZacQWmEctTeUqyfSU4SDEnEwckWxd92W2G",
  "secret_key": "ed25519:****"
}
```

#####  Mulai the validator node

Kembali ke folder awal
````
cd nearcore
````

```
target/release/neard run
```
Jika tidak terjadi error lanjutkan untuk membuat Systemd

* Siapkan Systemd
Command:
Kembali ke folder paling awal

```
sudo nano /etc/systemd/system/neard.service
```
Paste:

Jika anda mengunakan vps digital ocean silahkan menggunakan ini.

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

```
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
```

> Note: Ubah USER dengan paths kamu
> Cara keluar dari Vim :x lalu enter
> cara edit Vim tekan S lalu keluar dari edit esc
> lalu keluar dari Vim

Langkah Selanjutnya aktifkan systemctl

Command:

```
sudo systemctl enable neard
```
Command:

```
sudo systemctl start neard
```
Jika Anda perlu melakukan perubahan pada layanan karena kesalahan dalam file. Itu harus dimuat ulang:

```
sudo systemctl reload neard
```
###### Lihat logs
Command:

```
journalctl -n 100 -f -u neard
```

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

Buat Log dengan warna

Command:

```
sudo apt install ccze
```
Melihat Logs Dengan Warna

Command:

```
journalctl -n 100 -f -u neard | ccze -A
```
#### Menjadi Validator
Untuk menjadi validator dan masuk ke dalam set validator, minimal harus memenuhi kriteria keberhasilan.

* Node harus disinkronkan sepenuhnya
* `validator_key.json` harus ada di tempatnya
* Kontrak harus diinisialisasi dengan kunci_publik di `validator_key.json`
* Account_id harus disetel ke id kontrak staking pool
* Harus ada delegasi yang cukup untuk memenuhi harga kursi minimum. Lihat harga kursi [di sini](https://explorer.shardnet.near.org/nodes/validators).
* Proposal harus diajukan dengan melakukan ping ke kontrak
* Setelah proposal diterima, validator harus menunggu 2-3 epoch untuk masuk ke set validator
* Setelah di set validator, validator harus menghasilkan lebih dari 90% blok yang ditugaskan

Periksa status menjalankan node validator. Jika “Validator” muncul, kumpulan Anda dipilih dalam daftar validator saat ini.


## Let's go to challenge 3 🚀

[Mount your Staking Pool](./Halaman_3.md).
