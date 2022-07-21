# Stake Wars: Episode III. Challenge 002

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
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
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

You will see the following:

![rust](https://user-images.githubusercontent.com/55140596/180235181-db2fbc43-a4b7-49f8-ac6c-9fc2eb90af4f.png)


Pilih 1 dan tekan enter.

##### Source the environment
```
source $HOME/.cargo/env
```

#### Clone `nearcore` project from GitHub
Pertama, clone the [`nearcore` repository](https://github.com/near/nearcore).

```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```

#### Compile `nearcore` binary
Masuk ke folder nearcore 

````
cd nearcore
````
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
kembali ke folder awal
````
cd ..
````
next step

```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```
#### Check status 
pada isi config.json 
jika isi kosong `"tracked_shards": []` ( tambahkan angka 0)
dan dibagian archive": false, jika masih true edit menjadi false
edit mengunakan nano
isi config berada di 
cd .near
nano config.json
cari `"tracked_shards" dan archive":
edit dan save 
ctrl + x lalu y lalu enter


**IMPORTANT: NOT REQUIRED TO GET SNAPSHOT AFTER HARDFORK ON SHARDNET DURING 2022-07-18**

Install AWS Cli
```
sudo apt-get install awscli -y
```

Download snapshot (genesis.json)
```
// IMPORTANT: NOT REQUIRED TO GET SNAPSHOT AFTER HARDFORK ON SHARDNET DURING 2022-07-18
cd ~/.near
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json
```

If the above fails, AWS CLI may be oudated in your distribution repository. Instead, try:
```
pip3 install awscli --upgrade
```

#### Run the node
To start your node simply run the following command:

```
cd ~/nearcore
./target/release/neard --home ~/.near run
```

![img](./images/download.png)
The node is now running you can see log outputs in your console. Your node should be find peers, download headers to 100%, and then download blocks.

----



### Activating the node as validator
##### Authorize Wallet Locally
A full access key needs to be installed locally to be able to sign transactions via NEAR-CLI.


* You need to run this command:

```
near login
```

> Note: This command launches a web browser allowing for the authorization of a full access key to be copied locally.

1 – Copy the link in your browser


![img](./images/1.png)

2 – Grant Access to Near CLI

![img](./images/3.png)

3 – After Grant, you will see a page like this, go back to console

![img](./images/4.png)

4 – Enter your wallet and press Enter

![img](./images/5.png)


#####  Check the validator_key.json
* Run the following command:
```
cat ~/.near/validator_key.json
```


> Note: If a validator_key.json is not present, follow these steps to create one

Create a `validator_key.json` 

*   Generate the Key file:

```
near generate-key <pool_id>
```
<pool_id> ---> xx.factory.shardnet.near WHERE xx is you pool name

* Copy the file generated to shardnet folder:
Make sure to replace <pool_id> by your accountId
```
cp ~/.near-credentials/shardnet/YOUR_WALLET.json ~/.near/validator_key.json
```
* Edit “account_id” => xx.factory.shardnet.near, where xx is your PoolName
* Change `private_key` to `secret_key`

> Note: The account_id must match the staking pool contract name or you will not be able to sign blocks.\

File content must be in the following pattern:
```
{
  "account_id": "xx.factory.shardnet.near",
  "public_key": "ed25519:HeaBJ3xLgvZacQWmEctTeUqyfSU4SDEnEwckWxd92W2G",
  "secret_key": "ed25519:****"
}
```

#####  Start the validator node

```
target/release/neard run
```
* Setup Systemd
Command:

```
sudo vi /etc/systemd/system/neard.service
```
Paste:

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

> Note: Change USER to your paths

Command:

```
sudo systemctl enable neard
```
Command:

```
sudo systemctl start neard
```
If you need to make a change to service because of an error in the file. It has to be reloaded:

```
sudo systemctl reload neard
```
###### Watch logs
Command:

```
journalctl -n 100 -f -u neard
```
Make log output in pretty print

Command:

```
sudo apt install ccze
```
View Logs with color

Command:

```
journalctl -n 100 -f -u neard | ccze -A
```
#### Becoming a Validator
In order to become a validator and enter the validator set, a minimum set of success criteria must be met.

* The node must be fully synced
* The `validator_key.json` must be in place
* The contract must be initialized with the public_key in `validator_key.json`
* The account_id must be set to the staking pool contract id
* There must be enough delegations to meet the minimum seat price. See the seat price [here](https://explorer.shardnet.near.org/nodes/validators).
* A proposal must be submitted by pinging the contract
* Once a proposal is accepted a validator must wait 2-3 epoch to enter the validator set
* Once in the validator set the validator must produce great than 90% of assigned blocks

Check running status of validator node. If “Validator” is showing up, your pool is selected in the current validators list.


## Let's go to challenge 3 🚀

[Mount your Staking Pool](./003.md).