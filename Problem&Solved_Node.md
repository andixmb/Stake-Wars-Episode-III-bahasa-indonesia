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
