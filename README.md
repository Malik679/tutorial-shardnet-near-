## Useful links

Wallet: https://wallet.shardnet.near.org/

Explorer: https://explorer.shardnet.near.org/ 


### Create a wallet
https://wallet.shardnet.near.org/

# Near - Stakewars III

**Mission I - VI**

Validation form for tasks 5,6 and 7

Untuk task 5 anda harus membuat artikel tentang tutorial shardnet

Task 6 menjalankan PING tutorial ada di paling akhir

Task 7 membuat data science untuk staking

Form : https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform

## Persyaratan Perangkat Keras yang Direkomendasikan

- 4x CPUs; the faster clock speed the better
- 8GB RAM
- 500GB of storage (SSD or NVME)
- Permanent Internet connection (traffic will be minimal during devnet; 10Mbps will be plenty - for production at least 100Mbps is expected)

## SETUP NEAR-CLI (CHALLENGE 1)

Update, Download Pacakge and Install Python 
```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker protobuf-compiler libssl-dev pkg-config clang llvm cargo clang build-essential make

sudo apt install python3-pip 

USER_BASE_BIN=$(python3 -m site --user-base)/bin export PATH="$USER_BASE_BIN:$PATH"
```

Install Node JS and NPM
```bash
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash - 
sudo apt install build-essential nodejs 
PATH="$PATH"
```
Install NEAR CLI
``` bash
sudo npm install -g near-cli
```
Setup Environment
```bash
export NEAR_ENV=shardnet 
echo ‘export NEAR_ENV=shardnet’ >> ~/.bashrc
source ~/.bashrc
```
Install Cargo and Rust (Wait)
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

## SETUP WALLET DAN VALIDATOR(CHALLENGE 2)


Donwload Binaries
```bash
git clone https://github.com/near/nearcore 
cd nearcore 
git fetch 
git checkout 8448ad1ebf27731a43397686103aa5277e7f2fcf
cargo build -p neard --release --features shardnet
```

Cek Versi
```bash
~/nearcore/target/release/neard --version
```

Inisialisasi direktori kerja, hapus file lama dan unduh file konfigurasi baru dan genesis.json baru. Karena jaringan sulit bercabang baru-baru ini, tidak perlu mengunduh snapshot.
```bash
~/nearcore/target/release/neard --home ~/.near init --chain-id shardnet --download-genesis 
rm ~/.near/config.json ~/.near/genesis.json
```

```bash
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json 
wget -O ~/.near/genesis.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json
```

```bash
mv ~/.near/data ~/.near/data-fork1 && rm ~/.near/config.json && wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json && rm ~/.near/genesis.json && wget -O ~/.near/genesis.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json
```

Sekarang Anda perlu membuat dompet https://wallet.shardnet.near.org/ PERINGATAN! Jika kamu memiliki dompet sebelumnya ke hard fork, Anda mungkin perlu membuatnya kembali! Lupakan mnemonic lama dan lakukan semuanya dari awal.

After that, write in the CLI
```bash
near login
```
![login1](https://user-images.githubusercontent.com/65535542/180588477-38dbb5a3-b778-479e-ac14-4154f0113926.png)
Confirm connect wallet 
![login2](https://user-images.githubusercontent.com/65535542/180588548-a604d2e5-ead6-4d3d-abca-b6da17622bb8.png)
if it looks like the one above, don't panic, because it's already connected successfully
![login3](https://user-images.githubusercontent.com/65535542/180588586-a97d118f-7f2d-4f37-b903-187fc77fdcb1.png)
Enter your address 
![login3](https://user-images.githubusercontent.com/65535542/180588597-431ba975-f31a-4e10-9ca5-016d74176fc8.png)
Done
### Next Step
See the authorisation link and copy it into the browser window where you wallet is opened. After connecting a wallet, right the wallet name ("accountId".shardnet.near) in the CLI and confirm.
#Copy your wallet json and make some changes
```bash
cd ~/.near-credentials/shardnet/
cp wallet.json ~/.near/validator_key.json
nano validator_key.json
```
Ubah ID akun dan parameter Private Key yang sesuai, lalu keluar dari editor nano.
```bash
{ 
"account_id": "xx.factory.shardnet.near", 
"public_key":"ed25519:HeaBJ3xLgvZacQWmEctTeUqyfSU4SDEnEwckWxd92W2G", "secret_key": "ed25519:****" 
}
```
Buat file layanan (satu perintah)
```bash
sudo tee /etc/systemd/system/neard.service > /dev/null <<EOF 
[Unit] 
Description=NEARd Daemon Service 
[Service] 
Type=simple 
User=$USER
#Group=near 
WorkingDirectory=$HOME/.near
ExecStart=$HOME/nearcore/target/release/neard run 
Restart=on-failure 
RestartSec=30 
KillSignal=SIGINT 
TimeoutStopSec=45 
KillMode=mixed 
[Install] 
WantedBy=multi-user.target 
EOF
```
Sekarang mulai layanan dan lihat log, bahwa semuanya berfungsi dengan baik. Anda akan melihatnya mengunduh tajuk terlebih dahulu dan blok. Menunggu sinkronisasi penuh.
```bash
sudo systemctl daemon-reload 
sudo systemctl enable neard 
sudo systemctl start neard
journalctl -f -u neard
```
```bash
sudo apt install ccze
journalctl -n 100 -f -u neard | ccze -A
```
![SyncNode](https://user-images.githubusercontent.com/65535542/180588601-ded8a58b-67fa-4297-985e-0057bbc3d339.png)
Wait a few minutes until download and sync


## MOUNTING STAKING POOL (CHALLENGE 3)



Now let’s deploy a contract of our staking pool with 30 NEAR staked!
```bash
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "<pool id>", "owner_id": "<accountId>", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="<accountId>" --amount=30 --gas=300000000000000
```
Change "pool id", "accountId", "public key", "accountId" parameters here!
My example for this
```bash
near call cotote.factory.shardnet.near create_staking_pool '{"staking_pool_id": "coyote", "owner_id": "coyote.shardnet.near", "stake_public_key": "ed25519:6gp2bTuvLaSCpACDoPauqzDLGyvX1ALDr3B3tEHPGDqy", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="0xdexa.shardnet.near" --amount=30 --gas=300000000000000
```
Jika semuanya baik-baik saja, Anda akan melihat diri Anda dalam perintah dekat proposal. Mari kita lihat harga kursi di bagian bawah proposal near memerintah. Dan kemudian kita perlu mempertaruhkan. Ingatlah untuk mengatur lingkungan untuk shardnet!
```bash
near proposals
```
Change parametres for staking accordingly!
```bash
near call coyote.factory.shardnet.near  deposit_and_stake --amount 200 --accountId coyote.shardnet.near --gas=300000000000000
```
Dalam beberapa epoch, Anda akan dapat melihat diri Anda di penjelajah dan dengan mengetik
```bash
near validators current 
near validators next
```


## MONITORING NODE STATUS (CHALLENGE 4)

Pada bagian ini, kalian akan membuat monitoring node status untuk melihat versi node dan lain sebagainya.

1. Install jq

    ```bash
    sudo apt install curl jq
    ```

2. Cek versi node kalian

    ```bash
    curl -s http://127.0.0.1:3030/status | jq .version
    ```
    
    ![Screenshot_33](https://user-images.githubusercontent.com/35837931/180384432-a17d6a2c-d8ff-4980-aa22-535c405aab9c.png)

    
    - Cek Delegators dan Stake

        Seperti biasa, ganti `xx` dengan nama wallet kalian.

        ```bash
        near view xx.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId xx.shardnet.near

        ```
    - Cek Block Produced
        
        Ganti `xx` dengan nama wallet kalian.
        
        ```bash
        curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq  '.result.current_validators[] | select(.account_id | contains ("xx.factory.shardnet.near"))'
        ```
        
    - Cek Reason Validator Kicked
        
        Ganti `xx` dengan nama wallet kalian.
        
        ```bash
        curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("xx.factory.shardnet.near"))' | jq .reason
        ```

3. Cek Sinkronisasi

    Pastikan status `syncing` adalah `false` agar validators kalian tidak tertinggal block. Jika masih `true` maka node kalian masih belum sepenuhnya sinkron.

    ```
    curl -s http://127.0.0.1:3030/status | jq .sync_info
    ```








## MEMBUAT PING SETIAP 2 JAM (CHALLENGE 6)

```bash
mkdir $HOME/nearcore/logs
```
```bash
nano $HOME/nearcore/scripts/ping.sh
```
Edit "PoolID" && "AccountID"
```bash
#!/bin/sh
# Ping call to renew Proposal added to crontab
export NEAR_ENV=shardnet
export LOGS=$HOME/nearcore/logs
export POOLID="PoolID"
export ACCOUNTID="AccountID"
echo "---" >> $LOGS/all.log
date >> $LOGS/all.log
near call $POOLID.factory.shardnet.near ping '{}' --accountId $ACCOUNTID.shardnet.near --gas=300000000000000 >> $LOGS/all.log
near proposals | grep $POOLID >> $LOGS/all.log
near validators current | grep $POOLID >> $LOGS/all.log
near validators next | grep $POOLID >> $LOGS/all.log
```
```bash
contrab -e
```
```bash
0 */2 * * * sh $HOME/nearcore/scripts/ping.sh
```
cek logs
```bash
cat $HOME/nearcore/logs/all.log
```

# SPLIT REWARD KE MULTIPLE AKUN (CHALLENGE 8)
Install cargo and Rust in case you don't have it. This command is for Linux or MacOS.

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh

source $HOME/.cargo/env
```

Add the wasm32-unknown-unknown toolchain

```bash
rustup target add wasm32-unknown-unknown
```

Clone the project

```bash
git clone https://github.com/zavodil/near-staking-pool-owner
```

Compile smart contract

```bash
cd near-staking-pool-owner/contract
rustup target add wasm32-unknown-unknown
cargo build --target wasm32-unknown-unknown --release
```

Deploy smart contract on your owner account. Adjust the path to .wasm file if required.

replace xx with your wallet name

```bash
NEAR_ENV=shardnet near deploy xx.shardnet.near --wasmFile target/wasm32-unknown-unknown/release/contract.wasm
```

Initialize the smart contract picking accounts for splitting revenue.

Ganti xx dengan nama wallet kalian, xx1 bisa isi nama wallet saya(coyote.shardnet.near)/teman kalian, untuk xx2 bisa isi nama wallet kalian sendiri

```bash
CONTRACT_ID=xx.shardnet.near

# Change numerator and denomitor to adjust the % for split.
near call $CONTRACT_ID new '{"staking_pool_account_id": "xx.factory.shardnet.near", "owner_id":"xx.shardnet.near", "reward_receivers": [["xx1.shardnet.near", {"numerator": 3, "denominator":10}], ["xx2.shardnet.near", {"numerator": 70, "denominator":100}]]}' --accountId $CONTRACT_ID
```

Contoh 

```bash
CONTRACT_ID=coyote.shardnet.near

# Change numerator and denomitor to adjust the % for split.
near call $CONTRACT_ID new '{"staking_pool_account_id": "coyote.factory.shardnet.near", "owner_id":"coyote.shardnet.near", "reward_receivers": [["smartinvest.shardnet.near", {"numerator": 3, "denominator":10}], ["coyote.shardnet.near", {"numerator": 70, "denominator":100}]]}' --accountId $CONTRACT_ID
```

Wait until you start receiving rewards on your node staking pool. Do a withdraw of rewards.

xx ganti nama wallet kalian

```bash
CONTRACT_ID=xx.shardnet.near
NEAR_ENV=shardnet near call $CONTRACT_ID withdraw '{}' --accountId $CONTRACT_ID --gas 200000000000000
```


# useful commands


1. Cek proposal

```bash
near proposals | grep xx.factory.shardnet.near
```

2. Cek validator kalian apakah aktif atau tidak, jika tidak aktif tidak keluar output apapun (Check your validator whether it is active or not, if it is not active, no output comes out)

```bash
near validators current | grep xx.factory.shardnet.near
```

3. Cek next status validator kalian (check your next validator status)

```bash 
near validators next | grep xx.factory.shardnet.near
```

4. Mengecek log (Checking logs)

```bash 
journalctl -n 100 -f -u neard
```
Atau jika ingin berwarna jalankan command ini (Or if you want color run this command):


```bash
sudo apt install ccze
```
```bash
journalctl -n 100 -f -u neard | ccze -A
```

5. Cek delegator dan stake 

```bash
near view xx.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId xx.shardnet.near
```

6. Cek produced block dan chunk

```bash
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq  '.result.current_validators[] | select(.account_id | contains ("xx.factory.shardnet.near"))'
```
7. Cek alasan kenapa validator anda ter-kickout (Check the reason why your validator was kicked out)

```bash
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("xx.factory.shardnet.near"))' | jq .reason
```

8. Cek sinkronisasi (check sync)

```bash 
curl -s http://127.0.0.1:3030/status | jq .sync_info
```
