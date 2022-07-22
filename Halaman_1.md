# Stake Wars: Episode III. Challenge 001

Form for Stakewars submissions - Will update when new challenge :) https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform

List Challenge 
https://github.com/near/stakewars-iii/blob/main/challenges/challenge-summary.md


### Siapkan simpul Anda
#### Persyaratan Minimum Server
Silakan lihat persyaratan perangkat keras di bawah ini:

| Hardware       | Chunk-Only Producer  Specifications                                   |
| -------------- | ---------------------------------------------------------------       |
| CPU            | 4-Core CPU with AVX support                                           |
| RAM            | 8GB DDR4                                                              |
| Storage        | 500GB SSD                                                             |


#### Menyiapkan NEAR-CLI

NEAR-CLI adalah antarmuka baris perintah yang berkomunikasi dengan blockchain NEAR melalui panggilan prosedur jarak jauh (RPC):

* Setup dan Instalasi NEAR CLI
* Lihat Statistik Validator

Pertama, mari kita pastikan mesin linux up-to-date.
```
sudo apt update && sudo apt upgrade -y
```

##### Install developer tools, Node.js, and npm
Pertama, kita akan mulai dengan menginstal `Node.js` dan `npm`:
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
````
````
sudo apt install build-essential nodejs
````
````
PATH="$PATH"
````
Jika error saat penginstalan nodejs ulangi dari awal dan pilih Y ( Y besar)
Lalu next PATH

Check Versi `Node.js` and `npm` version:
```
node -v
```
> v18.x.x
```
npm -v
```
> 8.x.x

##### Install NEAR-CLI
Inilah Repositori Github untuk NEAR CLI.: https://github.com/near/near-cli. Untuk menginstal NEAR-CLI, ( kecuali Anda login sebagai root, tidak disarankan menggunakan `sudo` untuk menginstal NEAR-CLI sehingga biner dekat disimpan ke /usr/local/bin

```
sudo npm install -g near-cli
```
### Validator Stats

Sekarang setelah NEAR-CLI terinstal, mari kita uji CLI dan gunakan perintah berikut untuk berinteraksi dengan blockchain serta untuk melihat statistik validator. Ada tiga laporan yang digunakan untuk memantau status validator:


###### Environment
Network perlu diatur setiap kali shell baru diluncurkan untuk memilih jaringan yang benar.

Networks:
- GuildNet
- TestNet
- MainNet
- **Shardnet** (ini adalah jaringan yang akan kami gunakan untuk Stake Wars)

Command:
```
export NEAR_ENV=shardnet
```

Selanjutnya jalankan perintah ini untuk mengatur Near testnet Environment persisten:
```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
```

#### NEAR CLI Commands Guide:

## Bisa anda lewati dan melanjutkan ke step berikutnya

###### Proposals
Proposal oleh validator menunjukkan bahwa mereka ingin masuk ke set validator, agar proposal dapat diterima harus memenuhi harga kursi minimum.

Command:
```
near proposals
```

###### Validators Current
Ini menunjukkan daftar validator aktif di periode saat ini, jumlah blok yang dihasilkan, jumlah blok yang diharapkan, dan tarif online. Digunakan untuk memantau jika validator mengalami masalah.

Command:
```
near validators current
```

###### Validators Next
Ini menunjukkan validator yang proposalnya diterima satu epoch yang lalu, dan yang akan masuk ke set validator di epoch berikutnya.

Command:
```
near validators next
```

---


## Langkah Selanjutnya

[Setup and Run your Node](./Halaman_2.md)
