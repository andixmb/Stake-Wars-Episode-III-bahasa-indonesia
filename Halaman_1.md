# Stake Wars: Episode III. Challenge 001

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

Check `Node.js` and `npm` version:
```
node -v
```
> v18.x.x
```
npm -v
```
> 8.x.x

##### Install NEAR-CLI
Here's the Github Repository for NEAR CLI.: https://github.com/near/near-cli. To install NEAR-CLI, unless you are logged in as root, which is not recommended you will need to use `sudo` to install NEAR-CLI so that the near binary is saved to /usr/local/bin

```
sudo npm install -g near-cli
```
### Validator Stats

Now that NEAR-CLI is installed, let's test out the CLI and use the following commands to interact with the blockchain as well as to view validator stats. There are three reports used to monitor validator status:


###### Environment
The environment will need to be set each time a new shell is launched to select the correct network.

Networks:
- GuildNet
- TestNet
- MainNet
- **Shardnet** (this is the network we will use for Stake Wars)

Command:
```
export NEAR_ENV=shardnet
```

You can also run this command to set the Near testnet Environment persistent:
```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
```

#### NEAR CLI Commands Guide:

###### Proposals
A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.

Command:
```
near proposals
```

###### Validators Current
This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.

Command:
```
near validators current
```

###### Validators Next
This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.

Command:
```
near validators next
```

---


## We continue ?

[Setup and Run your Node](./002.md)
