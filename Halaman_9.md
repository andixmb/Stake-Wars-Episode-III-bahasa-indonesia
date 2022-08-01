# Stake Wars: Episode III. Challenge 009

* Rewards: 15 Unlocked NEAR Points

Tantangan ini memiliki tujuan:
* Pantau waktu aktif di atas 70% di ShardNet
* Buka port RPC 3030 untuk analitik / pelaporan

️Tip: Di MainNet, Uptime harus lebih dari 95%, atau Anda akan ditendang hingga 3 epoch kehilangan hadiah dan kemungkinan delegator.

Tahukah Anda? Jika validator mengeluarkan ping untuk bergabung dengan jaringan sebelum disinkronkan sepenuhnya, apakah itu dianggap sebagai serangan terhadap jaringan?

## Langkah

Kami telah meluncurkan papan skor validator baru untuk ShardNet tempat Anda dapat memantau waktu aktif dan metrik validator Anda: [ShardNet Uptime Leaderboard](https://openshards.io/shardnet-uptime-scoreboard/)

### Tujuan 1 - Memantau waktu aktif

* Periksa waktu aktif Anda saat ini dan kelola hingga di atas 70% di ShardNet
* Perbaiki masalah dengan menghasilkan potongan.
Lihat [panduan pemecahan masalah](https://github.com/near/stakewars-iii/blob/main/challenges/troubleshooting.md)
* Terapkan skrip pemantauan

### Objective 2 - Open Port 3030 for Diagnostic reporting

Check to see if PORT 3030 is open
```
sudo iptables -L | grep 3030
```
Open the port if not open
```
sudo iptables -A INPUT -p tcp --dport 3030 -j ACCEPT
```
Save the config for server restarts

You can use one of the 2 solutions:
##### Using iptables-persistent
```
sudo apt install iptables-persistent
```
or if already installed
```
sudo dpkg-reconfigure iptables-persistent
```

##### Using files
```
iptables-save > /etc/iptables/rules.v4
ip6tables-save > /etc/iptables/rules.v6
```

#### Validasi port terbuka dengan mengunjungi
http://<IP ANDA>:3030/status
**CATATAN:** ​​_Dalam beberapa kasus, port mungkin juga perlu dibuka di firewall penyedia cloud/pusat data Anda.
## Kriteria penerimaan:
* Validator melaporkan di atas 70%
* Port dapat diakses secara publik `http://<IP Address>:3030/status`

## Challenge submission

* Challenge URL: The link to your port 3030 port
* Challenge image: Screenshot of uptime from Leaderboard
