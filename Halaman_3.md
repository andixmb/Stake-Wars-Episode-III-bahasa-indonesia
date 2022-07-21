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
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "gateomega", "owner_id": "gateomega.shardnet.near", "stake_public_key": "ed25519:CiGdCnC98pBRq5dj6bw1r4RUpUC2k7NfMWQ6Qef5PQLi", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="gateomega.shardnet.near" --amount=551 --gas=300000000000000
````
edit 
