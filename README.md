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
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet (1)
### Jawaban
-

## Soal 2
bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise (2).
### Jawaban
-


## Soal 3
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden (3).
### Jawaban
-


## Soal 4
 Buat juga reverse domain untuk domain utama (4).
### Jawaban
-

## Soal 5
buatlah juga Berlint sebagai DNS Slave untuk domain utama WISE(5).
### Jawaban
-

## Soal 6
buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation (6).
### Jawaban
-


## Soal 7
buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden (7).
### Jawaban
-


### Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com (8).
#### Jawaban
-


### Soal 9
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home (9).
#### Jawaban
-

### Soal 10
Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com (10).

#### Jawaban
-

### Soal 11
Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja (11).

#### Jawaban
-

### Soal 12
Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache (12)

#### Jawaban
-

### Soal 13
Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js (13).

#### Jawaban
-

### Soal 14
Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500 (14)

#### Jawaban
-

### Soal 15
dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy (15)

#### Jawaban
-

### Soal 16
dan setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com (16).

#### Jawaban
-

### Soal 17
Karena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png. Bantulah Agent Twilight dan Organisasi WISE menjaga perdamaian! (17)

#### Jawaban
-

## Pembagian Tugas
| Nama                        | Nomor      |
|:---------------------------:|:----------:|
| Davian Benito               | 4 - 7      |
| Fitra Agung Diassyah Putra  | 8 - 17     |
| Yusron Nugroho Aji          | 1 - 3      |

## Revisi
8 - 17

## Kendala
Tidak ada
