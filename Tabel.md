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





# Table of Contents

| Challenges | Deskripsi                             | Tanggal Mulai | Tanggal Selesai | Link                                                              |
| ---------- | ------------------------------------- | ------------- | --------------- | ----------------------------------------------------------------- |
|   1        | Setup NEAR-CLI                        | 13 Juli 2022  | 11 Agustus 2022 |[Tutorial](https://github.com/Malik679/tutorial-shardnet-near-#setup-near-cli-challenge-1) |
|   2        | Setup Wallet dan Run Validator        | 13 Juli 2022  | 11 Agustus 2022 |[Tutorial](https://github.com/Malik679/tutorial-shardnet-near-#setup-wallet-dan-validatorchallenge-2) |
|   3        | Mounting Staking Pool                 | 13 Juli 2022  | 11 Agustus 2022 |[Tutorial](https://github.com/Malik679/tutorial-shardnet-near-#mounting-staking-pool-challenge-3) |
|   4        | Membuat Monitoring Node Status        | 13 Juli 2022  | 11 Agustus 2022 |[Tutorial](https://github.com/Malik679/tutorial-shardnet-near-#monitoring-node-status-challenge-4) |
|   5        | Membuat Tutorial Stakewars            | 15 Juli 2022  | 11 Agustus 2022 |-                                                                  |
|   6        | Membuat Ping Otomatis Setiap 2 Jam    | 19 Juli 2022  | 11 Agustus 2022 |[Tutorial](https://github.com/Malik679/tutorial-shardnet-near-#membuat-ping-setiap-2-jam-challenge-6) |
|   7        | Membuat Data Science untuk Staking    | 22 Juli 2022  | 07 September 2022 |-
|   8        | SPLIT REWARD KE MULTIPLE AKUN         | 26 Juli 2022  | 08 Agustus 2022 |[Tutorial](https://github.com/Malik679/tutorial-shardnet-near-/blob/main/README.md#split-reward-ke-multiple-akun-challenge-8)|



Summary for the point system:
* Delegated NEAR Points (DNP): at the end of the Stake Wars program, each Delegated NEAR Point (DNP) will be translated into 500 NEAR tokens delegated to your mainnet account for 1 year.
* Unlocked NEAR Points (UNP): at the end of the Stake Wars program, each Unlocked NEAR Point (UNP) will be translated into 1 unlocked NEAR token, granted to your mainnet account.

Form for Stakewars submissions - Will update when new challenge :): https://docs.google.com/forms/d/e/1FAIpQLScp9JEtpk1Fe2P9XMaS9Gl6kl9gcGVEp3A5vPdEgxkHx3ABjg/viewform

