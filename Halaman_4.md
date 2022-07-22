# Stake Wars: Episode III. Challenge 004

Siapkan Tools untuk memantau status Node. Instal dan gunakan RPC pada port 3030 untuk mendapatkan informasi berguna agar Node Anda tetap berfungsi.

## Link wallet dan explorer

Wallet: https://wallet.shardnet.near.org/

Explorer: https://explorer.shardnet.near.org/ 


### Monitor and make alerts 

Notifikasi email dapat membuat validator tetap aktif dan berjalan dengan lebih nyaman. Raih menjadi validator yang mengonfirmasi transaksi di testnet dan dapatkan >95% waktu aktif.

#### Log Files
File log disimpan di direktori ~/logs atau di systemd tergantung pada pengaturan Anda.

Seperti dokumen sebelumnya file log sudah anda buat

### Check Logs
Systemd Command:
```
journalctl -n 100 -f -u neard | ccze -A
```
### Check Status Node
````
journalctl -n 100 -f -u neard | grep "INFO stats"
````
**Log file sample:**

Validator | 1 validator

```
INFO stats: #85079829 H1GUabkB7TW2K2yhZqZ7G47gnpS7ESqicDMNyb9EE6tf Validator 73 validators 30 peers ⬇ 506.1kiB/s ⬆ 428.3kiB/s 1.20 bps 62.08 Tgas/s CPU: 23%, Mem: 7.4 GiB
```

* **Validator**: "Validator" akan menunjukkan bahwa Anda adalah validator aktif
* **73 validators**: Total 73 validator di jaringan
* **30 peers**: Saat ini Anda memiliki 30 peers. Anda membutuhkan setidaknya 3 peers untuk mencapai konsensus dan mulai memvalidas
* **#46199418**: block – blok – Pastikan blok bergerak

#### RPC
Selama port tersebut terbuka di firewall node. NEAR-CLI menggunakan panggilan RPC di belakang layar. Penggunaan umum untuk RPC adalah untuk memeriksa statistik validator, versi node dan untuk melihat saham delegator, meskipun dapat digunakan untuk berinteraksi dengan blockchain, akun, dan kontrak secara keseluruhan.

Temukan banyak perintah dan cara menggunakannya secara lebih rinci di sini:

https://docs.near.org/docs/api/rpc



Command:
```
sudo apt install curl jq
```
###### Common Commands:
####### Check Versi Node:
Command:
```
curl -s http://127.0.0.1:3030/status | jq .version
```
####### Check Delegators and Stake
Command:
```
near view <your pool>.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId <accountId>.shardnet.near
```
####### Check Reason Validator Kicked
Command:
```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("<POOL_ID>"))' | jq .reason
```
####### Check Blocks Produced / Expected
Command:
```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.current_validators[] | select(.account_id | contains ("POOL_ID"))'
```
####### Check Proposal
Command:
````
near proposals |grep <POOL ID>
````
####### Check Validators Current
Command:
````
near validators current |grep <POOL ID>
````
####### Check Validators Next
Command:
````
near validators next |grep <POOL ID>
````

## Let's go to challenge 5 🚀
[Run on multiple cloud providers](./Halaman_5.md).
