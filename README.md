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
- [Soal 11](#soal-11)
- [Soal 12](#soal-12)
- [Soal 13](#soal-13)
- [Soal 14](#soal-14)
- [Soal 15](#soal-15)
- [Soal 16](#soal-16)
- [Soal 17](#soal-17)
#### [Resource Soal](Resources)
#### [Pembagian Tugas](#pembagian-tugas-1)
#### [Revisi](#revisi-1)
#### [Kendala](#kendala-1)

## Soal 1
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet (1)
### Jawaban
- Pertama kami membuat topologi jaringan pada GNS seperti pada gambar, lalu diatur konfigurasi tiap node sesuai dengan IP kelompok kami yaitu 192.197 pada bagian `Edit Network Configuration`. 

Detailnya yaitu : 
1. Ostania
  ```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.197.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.197.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.197.3.1
	netmask 255.255.255.0
  ```
  
2. WISE 
  ```
auto eth0
iface eth0 inet static
	address 192.197.3.2
	netmask 255.255.255.0
	gateway 192.197.3.1
  ```
  
3. SSS
```
auto eth0
iface eth0 inet static
	address 192.197.1.2
	netmask 255.255.255.0
	gateway 192.197.1.1
```

4. Garden
```
auto eth0
iface eth0 inet static
	address 192.197.1.3
	netmask 255.255.255.0
	gateway 192.197.1.1
```

5. Berlint
```
auto eth0
iface eth0 inet static
	address 192.197.2.2
	netmask 255.255.255.0
	gateway 192.197.2.1
```

6. Eden
```
auto eth0
iface eth0 inet static
	address 192.197.2.3
	netmask 255.255.255.0
	gateway 192.197.2.1
```

Lalu tidak lupa untuk menghubungkan semua node ubuntu cabang dari Ostania agar dapat terhubung dengan internet dari Ostania. Caranya sebagai berikut : 
1. Pada Ostania dipanggil `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.197.0.0/16`. Lalu cek resolv.conf nya pada path `/etc/resolv.conf`. Didapatkan nameserver 192.168.122.1

2. Lalu pada tiap node ubuntu lainnya, spesifikkan nameserver dari Ostania tersebut pada file /etc/resolv.conf masing2 dengan cara `echo nameserver 192.168.122.1 > /etc/resolv.conf`


## Soal 2
bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise (2).
### Jawaban
- Pertama lakukan instalasi bind9 pada node WISE sebagai DNS utama dengan fungsi `apt-get update` dan `apt-get install bind9 -y`. 

Lalu ubah file berikut : 
```
echo 'zone "wise.E10.com" {
        type master;
        notify yes;
        also-notify { 192.197.2.2; };
        allow-transfer { 192.197.2.2; };
        allow-transfer { 192.168.2.3; };
        file "/etc/bind/wise/wise.E10.com";
};' > /etc/bind/named.conf.local
```
Kemudian buat direktori baru dan copy db ke dalam folder tersebut dan beri nama yang sesuai
```
//mkdir /etc/bind/wise
cp /etc/bind/db.local /etc/bind/wise/wise.E10.com
```

Lalu ubah isi dari file tersebut
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.E10.com. root.wise.E10.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      wise.E10.com.
@               IN      A       192.197.3.2     ; IP Wise
www             IN      CNAME   wise.E10.com.
//eden          IN      A       192.197.2.3     ; IP Eden
ns1             IN      A       192.197.2.3     ; IP Eden
eden            IN      NS      ns1
//www.eden      IN      CNAME   eden.wise.E10.com.
n2                  IN  NS        192.197.2.2   ; IP Berlint
operation           IN  NS        ns2
' > /etc/bind/wise/wise.E10.com
```
Terakhir lalukan restart bind9. `service bind9 restart`

Kemudian tinggal pada setiap node client, tambahkan `nameserver 192.197.3.2` pada file resolv.conf. Node tersebut kemudian seharusnya sudah dapat mengakses website

## Soal 3
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden (3).
### Jawaban
- Pertama buka file wise.E10.com dan edit sesuai dengan konfigurasi berikut : 
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eden.wise.E10.com. root.eden.wise.E10.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      eden.wise.E10.com.
@               IN      A       192.197.2.3     ; IP Eden
www             IN      CNAME   eden.wise.E10.com.
' > /etc/bind/wise/eden.wise.E10.com
```
Kemudian tambahkan konfigurasi ini pada file named.conf.local : 
```
echo 'zone "eden.wise.E10.com" {
        type master;
        file "/etc/bind/wise/eden.wise.E10.com";
};' >> /etc/bind/named.conf.local
```
Seharusnya, subdomain sudah dapat diakses di node client. 

## Soal 4
 Buat juga reverse domain untuk domain utama (4).
### Jawaban
- Tambahkan konfigurasi berikut pada file named.conf.local di WISE, dimana IP merupakan IP reverse dari sebenarnya : 
```
echo 'zone "3.197.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/3.197.192.in-addr.arpa";
};' >> /etc/bind/named.conf.local
```
Lalu copy file db.local ke dalam folder wise dan ubah namanya `cp /etc/bind/db.local /etc/bind/wise/3.197.192.in-addr.arpa`
Kemudian konfigur isi dari file tersebut sebagai berikut : 
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.E10.com. root.wise.E10.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
3.197.192.in-addr.arpa.  IN      NS      wise.E10.com.
2                        IN      PTR     wise.E10.com. ; Byte ke-4 WISE

' > /etc/bind/wise/3.197.192.in-addr.arpa
```
Lalu restart bind9
Pada node client, lakukan installasi dnsutils memakai `apt-get update` dan `apt-get install dnsutils -y`, kemudian coba tes memakai command berikut : `host -t PTR 192.197.3.2`. Harusnya akan menunjukkan nama dari website IP tersebut. 

## Soal 5
buatlah juga Berlint sebagai DNS Slave untuk domain utama WISE(5).
### Jawaban
- Modifikasi file named.conf.local pada WISE sesuai konfigurasi berikut : 
```
echo 'zone "wise.E10.com" {
        type master;
        notify yes;
        also-notify { 192.197.2.2; };
        allow-transfer { 192.197.2.2; };
        allow-transfer { 192.168.2.3; };
        file "/etc/bind/wise/wise.E10.com";
};' > /etc/bind/named.conf.local
```
Restart bind9, lalu pada node berlint, install bind9 kemudian edit file named.conf.local sebagai berikut : 
```
echo 'zone "wise.E10.com" {
    type slave;
    masters { 192.197.3.2; }; // Masukan IP EniesLobby tanpa tanda petik
    file "/var/lib/bind/wise.E10.com";
};' > /etc/bind/named.conf.local
service bind9 restart
```
restart bind9, Lalu pada node client, kita juga perlu menambahkan nameserver IP Berlint. Bila sudah semua, maka website seharusnya sudah dapat diakses melalui Berlint ataupun WISE. 

## Soal 6
buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation (6).
### Jawaban
- Buka file wise.E10.com lalu pada konfigurasi file wise.E10.com sebagai berikut : 
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.E10.com. root.wise.E10.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      wise.E10.com.
@               IN      A       192.197.3.2     ; IP Wise
www             IN      CNAME   wise.E10.com.
//eden          IN      A       192.197.2.3     ; IP Eden
ns1             IN      A       192.197.2.3     ; IP Eden
eden            IN      NS      ns1
//www.eden      IN      CNAME   eden.wise.E10.com.
n2                  IN  NS        192.197.2.2   ; IP Berlint
operation           IN  NS        ns2
' > /etc/bind/wise/wise.E10.com
```
Kemudian buka file named.conf.options dan edit menjadi seperti berikut : 
```
echo'options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
	
        // forwarders {
        //      0.0.0.0;
        // };
        //=====================================================================$       //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options
```
dan restart bind. Lalu konfigurasikan pada node Berlint di file named.conf.local sebagai berikut : 
```
echo 'zone "wise.E10.com" {
    type slave;
    masters { 192.197.3.2; };
    file "/var/lib/bind/wise.E10.com";
};' > /etc/bind/named.conf.local
```
Kemudian buat folder baru pada bind dan copykan db.local kesana dan ubah nama sebagai berikut : 
```
mkdir /etc/bind/operation
cp /etc/bind/db.local /etc/bind/operation/operation.wise.E10.com
nano /etc/bind/operation/operation.wise.E10.com
```
Lalu edit sesuai konfigurasi berikut pada file tersebut : 
```
echo '
        ;
; BIND data file for local loopback interface;
$TTL    604800
@       IN      SOA     operation.wise.E10.com. root.operation.wise.E10.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      operation.wise.E10.com.
@       IN      A       192.197.2.3     ; IP Eden
www     IN      CNAME   operation.wise.E10.com.
ns1       IN        A           192.197.2.3         ; IP Eden
strix     IN        NS  ns1

' > /etc/bind/operation/operation.E10.com
```
Restart bind9. Kemudian apabila kita testing pada node client, seharusnya sudah dapat mengakses web yang dibuat tersebut. 

## Soal 7
buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden (7).
### Jawaban
- Atur konfigurasi pada berlint sebagai berikut :
```
echo 'zone "operation.wise.E10.com" {
    type master;
    file "/var/lib/bind/strix.operation.wise.E10.com";
};' >> /etc/bind/named.conf.local
```
Lalu kita copykan db.local dan ubah namanya sesuai format sebagai berikut : 
```
cp /etc/bind/db.local /etc/bind/wise/strix.operation.wise.E10.com
```
Buka file tersebut dan edit sesuai konfigurasi berikut :
```
        ;
; BIND data file for local loopback interface;
$TTL    604800
@       IN      SOA     strix.operation.wise.E10.com. root.strix.operation.wise.E10.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      strix.operation.wise.E10.com.
@       IN      A       192.197.2.3     ; IP Eden
www     IN      CNAME   strix.operation.wise.E10.com.
```
Lalu kita restart bind9. 
Buka node client dan lakukan testing memanggil website yang telah dibuat, seharusnya website sudah dapat diakses beserta aliasnya. 


### Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com (8).
#### Jawaban
- Pertama dilakukan instalasi PHP, Apache2, Library Apache2 pada WISE dengan menjalankan command berikut : 
```
apt-get update
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y

apt-get install wget -y
apt-get install unzip -y
```
Lalu download resource yang diperlukan memakai command `wget` dan unzip file tersebut memakai command `unzip`. 
Kemudian buat folder baru dan copas semua isi dari file zip ke dalamnya. 
```
mkdir /var/www/wise.E10.com
cp ~/wise/home.html /var/www/wise.E10.com
cp ~/wise/index.php /var/www/wise.E10.com
```
Lalu copy file 000-default.conf kedalam sites-available dan ubah namanya
`cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/wise.E10.com.conf`
Kemudian atur konfigurasinya sesuai module dengan nama yang sesuai : 
![image](https://user-images.githubusercontent.com/71868354/198832322-e0115cd6-defa-47f6-bab7-21386cb26e87.png)
Aktifkan konfigurasi website dengan commmand `a2ensite wise.E10.com.conf` dan restart Apache2 dengan command `service apache2 restart`
Bila sudah maka kita dapat mengeceknya dengan melakukan testing pada node client. Sebelumnya kita harus menginstall Lynx pada node client tersebut dengan `apt-get install lynx -y`
Bila benar, seharusnya kita dapat mengecek hasil dari webserver tersebut. 

### Soal 9
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home (9).
#### Jawaban
- Edit file wise.E10.com.conf dan konfigurasikan sebagai berikut : 
```
 <Directory /var/www/wise.E10.com>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
 </Directory>
```
buat file .htaccess pada folder /var/www/wise.E10.com dan konfigurasikan sebagai berikut : 
![image](https://user-images.githubusercontent.com/71868354/198832577-09a116b7-7704-4ce2-8275-d46e633a6300.png)
Lalu aktifkan module rewrite dengan command `a2enmod rewrite` dan restart apache2 dengan command `service apache2 restart`. 
Bila selesai maka kita dapat melakukan testing di node client dengan cek command `lynx wise.E10.com/home`. Seharusnya isi website sudah dapat tertampilkan

### Soal 10
Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com (10).

#### Jawaban
- Pertama unzip file resource lalu buat directory baru dan copykan semua file tersebut kesana juga copykan file 000-default.conf ke dalam folder sites-available dan ubah namanya. 
```
mkdir /var/www/super.wise.E10.com
cp -r ~/eden.wise/error /var/www/wise.E10.com
cp -r ~/eden.wise/public /var/www/wise.E10.com
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/eden.wise.E10.com.conf
```
Buka file tersebut dan konfigurasikan sesuai berikut : 
![image](https://user-images.githubusercontent.com/71868354/198832322-e0115cd6-defa-47f6-bab7-21386cb26e87.png)
Aktifkan konfigurasi website dan restart apache2
```
a2ensite eden.wise.E10.com.conf
service apache2 restart
```
Setelah selesai, kita dapat melakukan testing di node client dan memanggil command berikut : 
```
lynx eden.wise.E10.com
lynx www.eden.wise.E10.com
```

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
