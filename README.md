Don't forget to follow my Twitter https://www.twitter.com/mbagolll

# Tutorial membuat vps di azure

yang anda perlukan adalah akun azure saya sarankan anda membuat akunnya terlebih dahulu https://azure.microsoft.com/en-us/free/
    
1. Pertama pilih virtual machines lalu klik create dan pilih paling atas


![Screenshot 2022-07-25 113523](https://user-images.githubusercontent.com/58283112/180699579-10bf6141-7713-470f-9d0c-2e048d1eb387.png)

2. pada tahap ini silahkan isi seperti yang ada di gambar, jenis autentikasi terserah anda mau pilih ssh/password, dan jangan lupa aktifkan port 22,443,80

![Screenshot 2022-07-25 114014](https://user-images.githubusercontent.com/58283112/180699966-d4b5f0c0-91c1-481a-860c-34a06c0c9a3c.png)

3. setelah itu untuk disk, manajemen, dll tidak perlu diganti langsung klik create/buat harga vps spek yang di pakai adalah 80$ jadi saya sarankan memakai akun trial yang ada saldo credit

4. karena disk untuk menjalankan node shardnet ini cukup besar saya juga akan memberi tutorial menambahkan disk
   -pertama ke beranda lalu klik nama vps anda
   -lalu klik tombol hentikan dan tunggu sampai proses selesai
   
![Screenshot 2022-07-25 114845](https://user-images.githubusercontent.com/58283112/180700802-c3c169e0-9022-4af9-bbfb-18fff6032cb5.png)
 
 5. pindah ke pengaturan lalu pilih disk dan klik nama disk

![Screenshot 2022-07-25 115251](https://user-images.githubusercontent.com/58283112/180701095-b6d12fda-ec14-4569-8a09-befc1f50487f.png)

lalu pilih ukuran+kinerja

![Screenshot 2022-07-25 115842](https://user-images.githubusercontent.com/58283112/180701813-bf1772cf-b21d-4bb5-b9a0-47eb1f13dbfd.png)

6. lalu pilih yang ukuran 500 gb tapi kalau sya disini memilih ukuran 1 tb agar performa lebih bagus, untuk tingkat kinerja pilih P30-5000 IOPS,200 Mbps lalu klik ubah ukuran

![Screenshot 2022-07-25 120121](https://user-images.githubusercontent.com/58283112/180702210-86e9dbf8-05fb-4e20-8a73-575a87d91fb2.png)

7. step terakhir silahkan ke beranda dan klik nama vps kalian lalu langsung saja klik start (jika tombol start tidak bisa di klik silahkan kalian refresh terlebih dahulu) setelah itu copy IPv4 kalian ke terminal, kalau saya pakai termius aplikasi bisa di cari di playstore 

SELESAI! JANGAN LUPA BERI BINTANG DAN FOLLOW  GITHUB SAYA

jangan lupa juga untuk isi Chunk-Only Producer Onboarding Form https://nearprotocol1001.typeform.com/to/Z39N7cU9

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


# useful commands

Ganti xx dengan nama wallet kalian

1. Cek proposal

```bash
near proposals | grep xx.factory.shardnet.near
```

2. Cek validator kalian apakah aktif atau tidak, jika tidak aktif tidak keluar output apapun

```bash
near validators current | grep xx.factory.shardnet.near
```

3. Cek next status validator kalian 

```bash 
near validators next | grep xx.factory.shardnet.near
```

4. Mengecek log

```bash 
journalctl -n 100 -f -u neard
```
Atau jika ingin berwarna jalankan command ini:

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
7. Cek alasan kenapa validator anda ter-kickout

```bash
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("xx.factory.shardnet.near"))' | jq .reason
```

8. Cek sinkronisasi

```bash 
curl -s http://127.0.0.1:3030/status | jq .sync_info
```
