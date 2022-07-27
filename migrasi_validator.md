# Migrasikan validator Anda ke Vm lain.

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
Pastikan pula anda memiliki akses kw wallet shardnet anda.

Simpan semua backup file ke notepad anda.

Saatnya menjalankan full node di mesin baru.

### 2. Jalankan full node ke virtual machine baru

Silahkan mengikuti panduan ini sampai halaman 2 [Setup and Run your Node](./Halaman_1.md)

### 3. Tunggu hingga Node penuh di mesin baru selesai dan selesai mendownload HEADERS DAN BLOCK.

Sebelum mematikan node dan validator di mesin lama cek ketinggian block pada node lama dan node baru.

check di node lama.
````
journalctl -n 100 -f -u neard | grep "INFO stats"
````



