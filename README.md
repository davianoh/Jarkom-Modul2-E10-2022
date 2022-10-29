# Jarkom-Modul2-E10-2022

| Nama                        | NRP        |
|:---------------------------:|:----------:|
| Davian Benito               | 5025201220 |
| Fitra Agung Diassyah Putra  | 5025201072 |
| Yusron Nugroho Aji          | 5025201138 |

#### [Soal](#soal)
- [Soal 1](#soal-1)
- [Soal 2](#soal-2)
- [Soal 3](#soal-3)
- [Soal 4](#soal-4)
- [Soal 5](#soal-5)
- [Soal 6](#soal-6)
- [Soal 7](#soal-7)
- [Soal 8](#soal-8)
- [Soal 9](#soal-9)
- [Soal 10](#soal-10)
#### [Resource Soal](Resources)
#### [Pembagian Tugas](#pembagian-tugas-1)
#### [Revisi](#revisi-1)
#### [Kendala](#kendala-1)

## Soal 1
Sebutkan web server yang digunakan pada "monta.if.its.ac.id"!
### Jawaban
- Gunakan display filter `tcp contains monta` untuk mendapatkan jawaban yang dicari
![image](https://user-images.githubusercontent.com/71868354/192100366-6d65200a-f857-4211-8569-1c9aac3ecfa2.png)
- DItemukan server yang digunakan `nginx/1.10.3\r\n`

## Soal 2
Ishaq sedang bingung mencari topik ta untuk semester ini , lalu ia datang ke website monta dan menemukan detail topik pada website “monta.if.its.ac.id” , judul TA apa yang dibuka oleh ishaq ?
### Jawaban
- Gunakan display filter `tcp contains detailTopik` untuk dapat menemukan judul detail topik yang dimaksud akan dilakukan search dengan urutan /index.php/topik/detailTopik/194 di website monta.if.its.ac.id
![image](https://user-images.githubusercontent.com/71868354/192100469-0b3a5501-646f-4f08-8829-1aa04b0e3c97.png)
- Topik Tugas Akhir : Evaluasi unjuk kerja User Space Filesystem (FUSE) oleh WAHYU SUADI yang didapatkan dari Request URI : /index.php/topik/detailTopik/194


## Soal 3
Filter sehingga wireshark hanya menampilkan paket yang menuju port 80!
### Jawaban
- Gunakan display filer `tcp.dstport == 80`
### Screenshot Pengerjaan
![image](https://user-images.githubusercontent.com/71868354/192100513-0beb7ebb-c15f-49bf-ad3f-f5d91fa4dee9.png)


## Soal 4
Filter sehingga wireshark hanya mengambil paket yang berasal dari port 21!
### Jawaban
- Gunakan display filer `tcp.srcport == 21 || udp.srcport == 21`
### Screenshot Pengerjaan
![image](https://user-images.githubusercontent.com/71868354/192100613-14379656-4d76-421b-9c24-03d8f45fadac.png)

## Soal 5
Filter sehingga wireshark hanya mengambil paket yang berasal dari port 443!
### Jawaban
- Gunakan display filer `tcp.srcport == 443`
### Screenshot Pengerjaan
![image](https://user-images.githubusercontent.com/71868354/192100670-12a3e0c1-cfd5-4049-9a48-0c2459a9c6eb.png)

## Soal 6
Filter sehingga wireshark hanya menampilkan paket yang menuju ke lipi.go.id !
### Jawaban
- Gunakan display filter `http.host==lipi.go.id`
### Screenshot Pengerjaan
![image](https://user-images.githubusercontent.com/71868354/192100687-ea1d7498-f2e5-4ede-8e9f-79b0facaaebe.png)


## Soal 7
Filter sehingga wireshark hanya mengambil paket yang berasal dari ip kalian!
### Jawaban
- Gunakan display filer `src host 192.168.37.181 / src host (IP)`
### Screenshot Pengerjaan
![image](https://user-images.githubusercontent.com/71868354/192100698-9d512f6c-ff11-4c7b-b7be-6c0fd6da5397.png)


### Soal 8 - 10
Di sebuah planet bernama Viltrumite, terdapat Kementerian Komunikasi dan Informatika yang baru saja menetapkan kebijakan baru. Dalam kebijakan baru tersebut, pemerintah dapat mengakses data pribadi masyarakat secara bebas jika memang dibutuhkan, baik dengan maupun tanpa persetujuan pihak yang bersangkutan. Sebagai mahasiswa yang sedang melaksanakan program magang di kementerian tersebut, kalian mendapat tugas berupa penyadapan percakapan mahasiswa yang diduga melakukan tindak kecurangan dalam kegiatan Praktikum Komunikasi Data dan Jaringan Komputer 2022. Selain itu, terdapat sebuah password rahasia (flag) yang diduga merupakan milik sebuah organisasi bawah tanah yang selama ini tidak sejalan dengan pemerintahan Planet Viltrumite. Tunggu apa lagi, segera kerjakan tugas magang tersebut agar kalian bisa mendapatkan pujian serta kenaikan jabatan di kementerian tersebut!

### Soal 8
Telusuri aliran paket dalam file .pcap yang diberikan, cari informasi berguna berupa percakapan antara dua mahasiswa terkait tindakan kecurangan pada kegiatan praktikum. Percakapan tersebut dilaporkan menggunakan protokol jaringan dengan tingkat keandalan yang tinggi dalam pertukaran datanya sehingga kalian perlu menerapkan filter dengan protokol yang tersebut.

#### Jawaban
- Gunakan display filer `tcp`, lalu dari sana dapat dilihat komunikasi banyak terjadi antara IP 127.0.0.1 dengan IP 127.0.1.1. 
![image](https://user-images.githubusercontent.com/71868354/192100725-4553b08f-9266-45c4-8a24-a8294a7a8ac1.png)

- Kemudian dapat kita follow tcp stream dan didapatkan sadapan message sebagai berikut : 
![image](https://user-images.githubusercontent.com/71868354/192100734-ff427fc9-be9f-45ad-bd9b-13cf258a195a.png)

### Soal 9
Terdapat laporan adanya pertukaran file yang dilakukan oleh kedua mahasiswa dalam percakapan yang diperoleh, carilah file yang dimaksud! Untuk memudahkan laporan kepada atasan, beri nama file yang ditemukan dengan format [nama_kelompok].des3 dan simpan output file dengan nama “flag.txt”.

#### Jawaban
- Dari sadapan message sebelumnya didapati bahwa pertukaran file ada di port 9002, sehingga dapat kita lakukan display filter `tcp.port == 9002 and tcp.len>0`

![image](https://user-images.githubusercontent.com/71868354/192100751-37ff34d9-5cf4-4375-aa8b-015ed9f6a7ca.png)

- Didapati 2 package yang penting adalah no.61 kemudian kita save data dari payload secara raw menjadi file.des3 untuk dilakukan deskripsi dengan openssl des3

![image](https://user-images.githubusercontent.com/71868354/192100765-e89ab62f-a5cf-462d-8ff1-01cc1755d352.png)


### Soal 10
Temukan password rahasia (flag) dari organisasi bawah tanah yang disebutkan di atas!
Note: Terkait soal nomor 9 dan 10, file yang didapatkan tidak perlu dikumpulkan, cukup tulis flag yang didapatkan ke dalam laporan kalian 🙏.

#### Jawaban
Password rahasianya adalah nakano karena saya udah mbaca manganya sampai chapter 100an :)
Dari situ, file.des3 kita dekrip memakai openssl des3 dengan password yang diketahui sebagai berikut :
![image](https://user-images.githubusercontent.com/71868354/192100790-e2f2002e-2872-42e0-bca0-35af40199de6.png)

Flag nya adalah `JaRkOm2022{8uK4N_CtF_k0k_h3h3h3}`

## Pembagian Tugas
| Nama                        | Nomor      |
|:---------------------------:|:----------:|
| Davian Benito               | 8, 9, 10   |
| Fitra Agung Diassyah Putra  | 3, 4, 5, 6 |
| Yusron Nugroho Aji          | 1, 2, 7    |

## Revisi
Tidak ada

## Kendala
Tidak ada
