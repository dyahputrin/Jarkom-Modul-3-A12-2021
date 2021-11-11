# Jarkom-Modul-3-A12-2021
Jarkom_Modul3_Lapres_A12

**Praktikum Modul 3 - Jaringan Komputer 2021**
**A12**
-   Fiqey Indriati Eka Sari (05111940000015)
-   Dyah Putri Nariswari (05111940000047)
-   Muhammad Farrel Abhinaya (05111940000173)

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

Setelah install lynx, pada Loguetown lakukan command ```export http_proxy="http://10.5.2.3:5000”```. Lalu, periksa apakah konfigurasi telah berhasil dengan command ```env | grep -i proxy```
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














