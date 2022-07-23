Don't forget to follow my Twitter https://www.twitter.com/mbagolll

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

## Instalasi

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
Now let’s deploy a contract of our staking pool with 30 NEAR staked!
```bash
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "<pool id>", "owner_id": "<accountId>", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="<accountId>" --amount=30 --gas=300000000000000
```
Change "pool id", "accountId", "public key", "accountId" parameters here!
My example for this
```bash
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "0xdexa", "owner_id": "0xdedxa.shardnet.near", "stake_public_key": "ed25519:FS6KjVhKNaZHnwrSerQBPLJLJGYZkH66j9FNbALHWYz5", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="0xdexa.shardnet.near" --amount=30 --gas=300000000000000
```
Jika semuanya baik-baik saja, Anda akan melihat diri Anda dalam perintah dekat proposal. Mari kita lihat harga kursi di bagian bawah proposal near memerintah. Dan kemudian kita perlu mempertaruhkan. Ingatlah untuk mengatur lingkungan untuk shardnet!
```bash
near proposals
```
Change parametres for staking accordingly!
```bash
near call 0xdexa.factory.shardnet.near  deposit_and_stake --amount 1200 --accountId 0xdexa.shardnet.near --gas=300000000000000
```
Dalam beberapa epoch, Anda akan dapat melihat diri Anda di penjelajah dan dengan mengetik
```bash
near validators current 
near validators next
```

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
*/5 * * * * sh $HOME/nearcore/scripts/ping.sh
```
cek logs
```bash
cat $HOME/nearcore/logs/all.log
```
