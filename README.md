# Jarkom-Modul-3-A12-2021
Jarkom_Modul3_Lapres_A12

**Praktikum Modul 3 - Jaringan Komputer 2021**
**A12**
-   Fiqey Indriati Eka Sari (05111940000015)
-   Dyah Putri Nariswari (05111940000047)
-   Muhammad Farrel Abhinaya (05111940000173)
---

Luffy yang sudah menjadi Raja Bajak Laut ingin mengembangkan daerah
kekuasaannya dengan membuat peta seperti berikut:

![](./media/image23.png)


Setting konfigurasi masing-masing node sebagai berikut:

**Foosha**
```

auto eth0

iface eth0 inet dhcp

auto eth1

iface eth1 inet static

address 10.5.1.1

netmask 255.255.255.0

auto eth2

iface eth2 inet static

address 10.5.2.1

netmask 255.255.255.0

auto eth3

iface eth3 inet static

address 10.5.3.1

netmask 255.255.255.0
```

**Loguetown**
```

auto eth0

iface eth0 inet static

address 10.5.1.2

netmask 255.255.255.0

gateway 10.5.1.1
```

**Alabasta**
```

auto eth0

iface eth0 inet static

address 10.5.1.3

netmask 255.255.255.0

gateway 10.5.1.1
```

**EniesLobby**
```

auto eth0

iface eth0 inet static

address 10.5.2.2

netmask 255.255.255.0

gateway 10.5.2.1
```

**Water7**
```

auto eth0

iface eth0 inet static

address 10.5.2.3

netmask 255.255.255.0

gateway 10.5.2.1
```

**Jipangu**
```

auto eth0

iface eth0 inet static

address 10.5.2.4

netmask 255.255.255.0

gateway 10.5.2.1
```

**Skypie**
```

auto eth0

iface eth0 inet static

address 10.5.3.2

netmask 255.255.255.0

gateway 10.5.3.1
```

**TottoLand**
```

auto eth0

iface eth0 inet static

address 10.5.3.3

netmask 255.255.255.0

gateway 10.5.3.1
```

Lalu pada node Foosha tuliskan command **iptables -t nat -A POSTROUTING
-o eth0 -j MASQUERADE -s 10.5.0.0/16** pada .**bashrc** dan tuliskan
command echo nameserver 192.168.122.1 \> /etc/resolv.conf pada node
lainnya.

-   **EniesLobby** sebagai DNS Server, **Jipangu** sebagai DHCP Server,
    **Water7** sebagai Proxy Server

### **1.1 Pada EniesLobby**

Kita akan membuat node EniesLobby sebagai DNS server.

### **1.1.A Instalasi Bind**

Buka EniesLobby dan buat script.sh untuk mempermudah update dan
instalasi bind9 pada EniesLobby

![](./media/image27.png)


### **1.1.B Pembuatan Domain**

Pada sesilab ini kita akan membuat domain franky.a12.com sesuai modul 2

-   Lakukan perintah pada EniesLobby. Isikan seperti berikut: nano
     /etc/bind/named.conf.local

-   Isikan configurasi domain franky.a12.com sesuai dengan syntax
     berikut:
  ```

 zone \"franky.a12.com\" {

 type master;

 file \"etc/bind/kaizoku/franky.a12.com\";

 };
 ```

 ![](./media/image19.png)
 

-   Buat folder kaizoku di dalam /etc/bind: mkdir /etc/bind/kaizoku

-   Copykan file db.local pada path /etc/bind ke dalam folder kaizoku
     yang baru saja dibuat dan ubah namanya menjadi franky.a12.com

 cp /etc/bind/db.local /etc/bind/kaizoku/franky.a12.com

 Kemudian buka file franky.a12.com dan edit seperti gambar berikut
 dengan IP EniesLobby vimmasing-masing kelompok: nano
 /etc/bind/kaizoku/franky.a12.com

 ![](./media/image13.png)
 

-   Restart bind9 dengan perintah

 service bind9 restart

 ATAU

 named -g //Bisa digunakan untuk restart sekaligus debugging

### **1.2 Pada Jipangu**

Kita akan membuat node Jipangu sebagai DHCP server.

### **1.2.A Instalasi DHCP Server**

Buka Jipangu dan buat script.sh untuk mempermudah update dan instalasi
isc-dhcp-server pada node Jipangu

![](./media/image9.png)


Lalu dijalankan script.sh untuk instalasi

### **1.2.B Konfigurasi DHCP Server**

-   Buka file konfigurasi interface dan edit file konfigurasi
     isc-dhcp-server pada /etc/default/isc-dhcp-server

-   Kita akan memilih interface eth0 untuk diberikan layanan DHCP yakni
     pada DHCP Relay

 ![](./media/image22.png)
 

### **1.3 Pada Water7**

Kita akan membuat node Water7 sebagai Proxy server.

### **1.3.A Instalasi dan Jalankan Squid**

Buka Water7 dan buat script.sh untuk mempermudah update dan instalasi
squid pada node Water7

![](./media/image8.png)


Selanjutnya, jalankan script.sh untuk instalasi dan start service

### **1.3.B Konfigurasi Proxy Server**

-   Backup terlebih dahulu file konfigurasi default yang disediakan
     Squid.

mv /etc/squid/squid.conf /etc/squid/squid.conf.bak

-   Buat konfigurasi Squid baru Pada file /etc/squid/squid.conf

-   Kemudian, pada file config yang baru, masukkan script
     :![](./media/image17.png)
     

-   Restart squid dengan cara mengetikkan perintah:

 service squid restart

### Soal 2 <br>

-   **Foosha** sebagai DHCP Relay

**Jawaban** <br>
**Pada Foosha**

1.  Buat script.sh untuk mempermudah selama pengerjaan, dengan instalasi
     isc-dhcp-relay

![](./media/image20.png)


2.  Jalankan script.sh

3.  Atur /etc/default/isc-dhcp-relay dengan Server berisi IP EniesLobby
     selaku DHCP Server, INTERFACES eth1 eth2 eth3 dikarenakan
     interfaces yang memerlukan dari router Foosha di Switch1, Switch2,
     Switch3.

![](./media/image28.png)


4.  Jalankan service isc-dhcp-relay start

5.  Dan cek status dengan service isc-dhcp-relay status

### Soal 3-4 <br>
Client yang melalui Switch1 mendapatkan range IP dari \[prefix
IP\].1.20 - \[prefix IP\].1.99 dan \[prefix IP\].1.150 - \[prefix
IP\].1.169 & Client yang melalui Switch3 mendapatkan range IP dari
\[prefix IP\].3.30 - \[prefix IP\].3.50

**Jawaban** <br>
-   Buka file konfigurasi DHCP dengan perintah nano /etc/dhcp/dhcpd.conf

-   Edit file konfigurasi isc-dhcp-server pada /etc/dhcp/dhcpd.conf
     dengan menambahkan option router dengan subnet gateway dari
     interface yang melewati Switch 2

-   Tambahkan script berikut
```

 subnet 10.5.1.0 netmask 255.255.255.0 {

 range 10.5.1.20 10.5.1.99;

 range 10.5.1.150 10.5.1.169;

 option routers 10.5.1.1;

 option broadcast-address 10.5.1.255;

 option domain-name-servers 10.5.2.2

 default-lease-time 360;

 max-lease-time 7200;

 }

 subnet 10.5.2.0 netmask 255.255.255.0 {

 }

 subnet 10.5.3.0 netmask 255.255.255.0 {

 range 10.5.3.30 10.5.3.50;

 option routers 10.5.3.1;

 option broadcast-address 10.5.3.255;

 option domain-name-servers 10.5.2.2;

 default-lease-time 720;

 max-lease-time 7200;

 }
 ```

-   ![](./media/image11.png)


-   Cek dan jalankan dhcp-server
    
    ![](./media/image14.png)
    ![](./media/image31.png)
     

-   Client yang melalui Switch 1 telah mendapatkan IP DHCP yang sesuai
     range yaitu range IP dari \[prefix IP\].1.20 - \[prefix IP\].1.99
     dan \[prefix IP\].1.150 - \[prefix IP\].1.169

 ![](./media/image36.png)
 

-   Client yang melalui Switch 3 telah mendapatkan IP DHCP yang sesuai
     range yaitu range IP dari \[prefix IP\].1.30 - \[prefix IP\].1.50

### Soal 5 <br>
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung
dengan internet melalui DNS tersebut.

**Jawaban** <br>
1.  DNS server harus disetting **DNS Forwarde**r. Di enies di setting
     forwarder /etc/bind/named.conf.options

 ![](./media/image10.png)
 

2.  Testing di LogueTown sebagai Client

 ![](./media/image37.png)
 

### Soal 6 <br>
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

**Jawaban** <br>
Pada node Jipangu, mengubah default-lease-time menjadi 360 dan 720 (dalam seconds) dan max-lease-time 7200 (dalam seconds) pada /etc/dhcp/dhcpd.conf

<img width="419" alt="Screenshot 2021-11-11 102914" src="https://user-images.githubusercontent.com/71380876/141273498-25366374-4758-4fd3-8de2-771797bae056.png">

----------------------------------------

### Soal 7 <br>
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69

**Jawaban** <br>
Pada Skypie, mengubah /etc/network/interfaces sebagai berikut<br>
<img width="421" alt="Screenshot 2021-11-11 103908" src="https://user-images.githubusercontent.com/71380876/141275097-888a9de4-c04d-47c6-b6b4-c536c73e13f7.png">

Pada Jipangu, menambahkan konfigurasi seperti berikut pada /etc/dhcp/dhcpd.conf
```
host Skypie {
  hardware ethernet de:e3:b3:35:e7:6b;
  fixed-address 10.5.3.69
}
```
Restart dengan command ```service isc-dhcp-server restart```

Lalu, stop dan start kembali Skypie pada GNS3 dan cek menggunakan command ```ip a```
<img width="420" alt="Screenshot 2021-11-11 104556" src="https://user-images.githubusercontent.com/71380876/141276113-fae18d7f-f9aa-4fdf-b15b-ec42c92b1dd6.png">

---------------------------------------------------

### Soal 8
Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

**Jawaban**
Pada EniesLobby tambahkan konfigurasi sebagai berikut pada /etc/bind/named.conf.local
![1636458721549](https://user-images.githubusercontent.com/71380876/141279228-0328312a-0896-4606-82a1-b4ba2e4a8d42.png)

Lalu, buat directory dengan command ```mkdir /etc/bind/kaizoku```

Edit konfigurasi pada /etc/bind/kaizoku/jualbelikapal.a12.com
![1636458721593](https://user-images.githubusercontent.com/71380876/141279447-6325452c-059d-49b9-8122-f05cc517e9a3.png)

Restart bind9 dengan command ```service bind9 restart```

Pada Water7, install squid dengan command ```apt-get install squid -y```. Lalu, start dengan command ```service squid start```.

Setelah itu, buat backup files (move and rename) squid.conf dengan command ```mv /etc/squid/squid.conf /etc/squid/squid.conf.bak```

Pada /etc/squid/squid.conf ubah sebagi berikut dan tambahkan ```http_access allow all```
![1636458721668](https://user-images.githubusercontent.com/71380876/141280770-9a071cce-06e5-4b93-93e6-43854347363e.png)

Restart squid dengan command ```service squid restart```

Setelah install lynx, pada Loguetown lakukan command ```export http_proxy="http://10.5.2.3:5000???```. Lalu, periksa apakah konfigurasi telah berhasil dengan command ```env | grep -i proxy```
<img width="385" alt="Screenshot 2021-11-11 112702" src="https://user-images.githubusercontent.com/71380876/141282350-ad6857b0-d108-414c-bede-df33d36b91b4.png">

--------------------------------------

### Soal 9
Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

**Jawaban**
Pada Water7, install apache2 dengan command ```apt-get install apache2 -y```. Setelah itu, tuliskan command ```htpasswd -c /etc/squid/passwd luffybelikapala12``` dan
```htpasswd -c /etc/squid/passwd zorobelikapala12```. Lalu, masukkan password nya yang sesuai. Setelah itu restart squid nya.<br>

![1636458721795](https://user-images.githubusercontent.com/71380876/141284205-a998b519-70f3-4762-879b-057743292bdc.png)

Pada /etc/squid/squid.conf tambahkan kofigurasi sebagai berikut <br>

![1636458721723](https://user-images.githubusercontent.com/71380876/141284083-ebc85a79-cebd-4dcb-84dd-f6203a803778.png)

restart squid

kemudian lakukan testing

![image](https://user-images.githubusercontent.com/81466736/141648001-a50e17ab-7902-40f3-9cc5-7c95f8782c2b.png)

### soal 10 
Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum???at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

Buat file baru bernama acl.conf di folder squid <br>
```vim /etc/squid/acl.conf```

Tambahkan:
``acl AVAILABLE_WORKING_1 time MTWH 07:00-11:00``

``acl AVAILABLE_WORKING_2 time TWHF 17:00-23:59``

``acl AVAILABLE_WORKING_3 time WHFA 00:00-03:00``

Dan edit seperti gambar di bawah :

![image](https://user-images.githubusercontent.com/81466736/141642882-adc9ab24-64e5-4c80-9a70-f18b3452a049.png)


Edit dan tambahkan 

``http_access allow USERS AVAILABLE_WORKING_1

http_access allow USERS AVAILABLE_WORKING_2

http_access allow USERS AVAILABLE_WORKING_3

http_access deny all``


di  file etc/squid/squid.conf menjadi:

![image](https://user-images.githubusercontent.com/81466736/141642914-b1d2e10f-f0f6-48dc-8133-73dc188df35e.png)

Restart squid <br>
``Service squid Restart``

Testing dengan membuka http://its.ac.id menggunakan lynx.

 
date -s "11 NOV 2021 02:00:00"   (termasuk watu yang tidak bisa mengakses internet) <br>
date -s "11 NOV 2021 04:00:00"   (termasuk watu yang tidak bisa mengakses internet)

![image](https://user-images.githubusercontent.com/81466736/141642936-bb64b13b-6824-455d-895c-51376ba7e5f9.png)

### soal 11 
Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

Pada enies lobby<br>
Edit file /etc/bind/named.conf.local

``Vim /etc/bind/named.conf.local``

![image](https://user-images.githubusercontent.com/81466736/141642959-cf6808b7-756d-4ff7-9e01-a360290d2942.png)

Buat folder baru bernama kaizoku<br>
``Mkdir /etc/bind/kaizoku``


Copy kan db.local ke dalam folder kaizoku dan ubah nama nya menjadi super.franky.a12.com

``cp /etc/bind/db.local /etc/bind/kaizoku/super.franky.a12.com``

Dan edit file /etc/bind/kaizoku/super.franky.a12.com

``Vim /etc/bind/kaizoku/super.franky.a12.com``

![image](https://user-images.githubusercontent.com/81466736/141643204-77c44499-980f-479b-9252-61fd274a72e9.png)

Restart bind9

Pada skypie Install aplikasi apache, PHP, dan libapache2-mod-php7.0.

lakukan apt-get update sebelumnya, lalu install aplikasi di atas

``apt-get install apache2 -y

apt-get install php -y

apt-get install libapache2-mod-php7.0 -y``



masuk ke dalam directory /etc/apache2/sites-available :

``cd /etc/apache2/sites-available.``

Copy file 000-default.conf dan ubah nama menjadi file super.franky.a12.com.conf.

``cp 000-default.conf super.franky.a12.com.conf``

Edit file franky.a12.com.conf seperti gambar berikut ini:

![image](https://user-images.githubusercontent.com/81466736/141642987-7a0f4d10-1a70-4c69-a3f6-345fa7f4026d.png)


pada directory /var/www.

``cd /var/www``

Download file zip yang telah diberikan : <br>
``wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip dan lakukan unzip``

``unzip super.franky.zip``

Rename folder super.franky menjadi super.franky.a12.com

``mv super.franky super.franky.a12.com``

Aktifkan konfigurasi pada franky.a12.com dengan command:

``a2ensite super.franky.a12.com``

![image](https://user-images.githubusercontent.com/81466736/141643052-233a2997-147d-4f33-a2aa-d9c4c368d011.png)

lakukan restart pada apache

``service apache2 restart``

Pada water 7 <br>
Buat file bernama restrict-sites.acl di folder squid.<br>
``vim /etc/squid/restrict-sites.acl``
tambahkan<br>
``www.google.com``

Kemudian edit file /etc/squid/squid.conf menjadi sebagai berikut

![image](https://user-images.githubusercontent.com/81466736/141643066-c4b671b7-0543-47c0-bdda-5ad96ea89ad2.png)

Coba membuka google.com dengan lynx di loguetown<br>
(dapat mengarahkan ke super.franky.a12.com tetapi tidak dapat mengakses super.franky.a12.com)

![image](https://user-images.githubusercontent.com/81466736/141648027-0a36ceb7-1249-443b-9fda-f7ae677b161c.png)


### soal 12 dan 13. 
Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps. Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya

Di water 7

Buat file acl-bandwidth.conf

``vim /etc/squid/acl-bandwidth.conf``
 
Dan tambahkan konfigurasi seperti berikut:

acl download url_regex -i \.jpg$ \.png$

auth_param basic program /user/lib/squid/basic_ncsa_auth /etc/squid/passwd<br>
acl luffy proxy_auth luffybelikapala12<br>
acl zoro proxy_auth zorobelikapala12<br>

delay_pools 2<br>
delay_class 1 1<br>
delay_parameters 1 1250/1250<br>
delay_access 1 allow luffy<br>
delay_access 1 deny zoro<br>
delay_access 1 allow download<br>
delay_access 1 deny all<br>

delay_class 2 1<br>
delay_parameters 2 -1/-1<br>
delay_access 2 allow zoro<br>
delay_access 2 deny luffy<br>
delay_access 2 deny all<br>


Buka  /etc/squid/squid.conf

Dan masukkan :

``include /etc/squid/acl-bandwith.conf ``

seperti gambar di bawah

![image](https://user-images.githubusercontent.com/81466736/141643119-6a31cb45-d174-4fdf-bc71-42b880e3b9be.png)


Restart squid 

``Service restart squid``

testing 

saat menngunakan user luffy

![image](https://user-images.githubusercontent.com/81466736/141648156-53cb7b5e-9be1-472e-a0d3-2f7c9fb5e7db.png)

saat menggunakan user zoro

![image](https://user-images.githubusercontent.com/81466736/141648141-d563b375-4f6b-4903-a75a-765f2aaf39f3.png)


### Kendala
1. saat nomor 11 tidak dapat membuka web super.franky.a12.com
2. testing untuk nomor 12 dan 13 menggunakan website lain karena kendala nomor 1


### Pembagian Tugas
-   Fiqey Indriati Eka Sari (05111940000015)  nomor 1-5
-   Dyah Putri Nariswari (05111940000047)     nomor 6-9
-   Muhammad Farrel Abhinaya (05111940000173) nomor 10-13
