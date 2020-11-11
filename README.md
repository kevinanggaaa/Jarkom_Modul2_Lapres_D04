# Jarkom_Modul2_Lapres_D04
## SOAL
![image](https://user-images.githubusercontent.com/60419316/98804307-99b5b600-2448-11eb-9010-68e7223879b8.png)  
Semeru adalah salah satu gunung yang terkenal di Jawa Timur. Bibah adalah salah satu juru kunci
Semeru. Bibah ingin menyebarkan keindahan Semeru pada dunia sehingga dia membeli 3 buah server
yang berada di MALANG, MOJOKERTO dan PROBOLINGGO. Server MALANG akan digunakan
sebagai DNS Server Master, MOJOKERTO akan digunakan sebagai DNS Server Slave dan
PROBOLINGGO akan digunakan sebagai Web Server. Selain 3 server terdapat 2 klien yang digunakan
untuk testing oleh Bibah yaitu GRESIK dan SIDOARJO. Untuk menyambungkan semua jaringan
tersebut Bibah memberi router di SURABAYA.

Kalian diminta untuk membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw yang
memiliki (2) alias http://www.semeruyyy.pw, dan (3) subdomain http://penanjakan.semeruyyy.pw
yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO serta dibuatkan (4)
reverse domain untuk domain utama. Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan
(5) DNS Server Slave pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru
pada Website. Selain website utama Bibah juga meminta dibuatkan (6) subdomain dengan alamat
http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP
Server PROBOLINGGO. Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota
komunitas sehingga dia meminta dibuatkan (7) subdomain dengan nama
http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO.

Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server. (8)
Domain http://semeruyyy.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw. Awalnya web
dapat diakses menggunakan alamat http://semeruyyy.pw/index.php/home. Karena dirasa alamat urlnya
kurang bagus, maka (9) diaktifkan mod rewrite agar urlnya menjadi http://semeruyyy.pw/home.
(10) Web http://penanjakan.semeruyyy.pw akan digunakan untuk menyimpan assets file yang
memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur
folder sebagai berikut:

/var/www/penanjakan.semeruyyy.pw  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;/public/javascripts  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;/public/css  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;/public/images  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;/errors  

(11) Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya
tidak dibolehkan. (12) Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada
folder /errors untuk mengganti error default 404 dari Apache. (13) Untuk mengakses file assets
javascript awalnya harus menggunakan url http://penanjakan.semeruyyy.pw/public/javascripts.
Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets
menjadi http://penanjakan.semeruyyy.pw/js.
Untuk web http://gunung.semeruyyy.pw belum dapat dikonfigurasi pada web server karena
menunggu pengerjaan website selesai. (14) sedangkan web http://naik.gunung.semeruyyy.pw
sudah bisa diakses hanya dengan menggunakan port 8888. DocumentRoot web berada pada
/var/www/naik.gunung.semeruyyy.pw. Dikarenakan web http://naik.gunung.semeruyyy.pw
bersifat private (15) Bibah meminta kamu membuat web http://naik.gunung.semeruyyy.pw agar
diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” supaya
aman dan tidak sembarang orang bisa mengaksesnya.
Saat Bibah mengunjungi IP PROBOLINGGO, yang muncul bukan web utama
http://semeruyyy.pw melainkan laman default Apache yang bertuliskan “It works!”. (16) Karena
dirasa kurang profesional, maka setiap Bibah mengunjungi IP PROBOLINGGO akan dialihkan
secara otomatis ke http://semeruyyy.pw. (17) Karena pengunjung pada
/var/www/penanjakan.semeruyyy.pw/public/images sangat banyak maka semua request gambar
yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.

## JAWABAN
### Membuat DNS
- Membuat topologi berdasarkan foto pada soal.

### Instalasi bind

- Buka *MALANG* dan update package lists dengan menjalankan command:

	```
	apt-get update
	```

- Setalah melakukan update, install aplikasi bind9 pada *MALANG* dengan perintah:

	```
	apt-get install bind9 -y
	```

### 1.2.B Pembuatan Domain
Pada Soal Shift 2 ini, Bibah ingin membuat website utama dengan alamat **semerud04.pw**.

- Lakukan perintah pada *MALANG*. Isikan seperti berikut:

  ```
   nano /etc/bind/named.conf.local
  ```

- Isikan configurasi domain **semerud04.pw** sesuai dengan syntax berikut:

  ```
  zone "semerud04.pw" {
  	type master;
  	file "/etc/bind/jarkom/semerud04.pw";
  };
  ```

- Buat folder **jarkom** di dalam **/etc/bind**

  ```
  mkdir /etc/bind/jarkom
  ```

- Copykan file **db.local** pada path **/etc/bind** ke dalam folder **jarkom** yang baru saja dibuat dan ubah namanya menjadi **semerud04.pw**

  ```
  cp /etc/bind/db.local /etc/bind/jarkom/semerud04.pw
  ```

- Kemudian buka file **semerud04.pw** dan edit seperti gambar berikut dengan IP *MALANG* , yaitu `10.151.79.42`

  ```
  nano /etc/bind/jarkom/jarkom2020.com
  ```

![konfig jarkom2020](gambar/3.png)

- Restart bind9 dengan perintah 

  ```
  service bind9 restart
  
  ATAU
  
  named -g //Bisa digunakan untuk restart sekaligus debugging
  ```



### 1.2.C Setting nameserver pada client

Domain yang kita buat tidak akan langsung dikenali oleh client oleh sebab itu kita harus merubah settingan nameserver yang ada pada client kita.

- Pada client *GRESIK* dan *SIDOARJO* arahkan nameserver menuju IP *MALANG* dengan mengedit file _resolv.conf_ dengan mengetikkan perintah 

	```
	nano /etc/resolv.conf
	```

![ping](gambar/4.png)

- Untuk mencoba koneksi DNS, lakukan ping domain **jarkom2020.com** dengan melakukan  perintah berikut pada client *GRESIK* dan *SIDOARJO*

  ```
  ping jarkom2020.com
  ```

![ping](gambar/5.png)



### 1.2.D Reverse DNS (Record PTR)

Jika pada pembuatan domain sebelumnya DNS server kita bekerja menerjemahkan string domain **jarkom2020.com** kedalam alamat IP agar dapat dibuka, maka Reverse DNS atau Record PTR digunakan untuk menerjemahkan alamat IP ke alamat domain yang sudah diterjemahkan sebelumnya.

- Edit file **/etc/bind/named.conf.local** pada *MALANG*

  ```
  nano /etc/bind/named.conf.local
  ```

- Lalu tambahkan konfigurasi berikut ke dalam file **named.conf.local**

  ```
  zone "71.151.10.in-addr.arpa" {
      type master;
      file "/etc/bind/jarkom/71.151.10.in-addr.arpa";
  };
  ```

![](gambar/6.png)

- Copykan file **db.local** pada path **/etc/bind** ke dalam folder **jarkom** yang baru saja dibuat dan ubah namanya menjadi **71.151.10.in-addr.arpa**

  ```
  cp /etc/bind/db.local /etc/bind/jarkom/71.151.10.in-addr.arpa
  ```

  *Keterangan 71.151.10 adalah 3 byte pertama IP MALANG yang dibalik urutan penulisannya*

- Edit file **71.151.10.in-addr.arpa** menjadi seperti gambar di bawah ini


![konfig](gambar/7.png)

- Kemudian restart bind9 dengan perintah 

  ```
  service bind9 restart
  ```

- Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client *GRESIK* 

  ```
  // Install package dnsutils
  // Pastikan nameserver telah dikembalikan ke settingan awal
  apt-get update
  apt-get install dnsutils
  
  //Kembalikan nameserver agar tersambung dengan MALANG
  host -t PTR "IP MALANG"
  ```

![host](gambar/8.png)



### 1.2.E Record CNAME
Record CNAME adalah sebuah record yang membuat alias name dan mengarahkan domain ke alamat/domain yang lain.

Langkah-langkah membuat record CNAME:

- Buka file **jarkom2020.com** pada server *MALANG* dan tambahkan konfigurasi seperti pada gambar berikut:


![DNS](gambar/9.png)



- Kemudian restart bind9 dengan perintah

  ```
  service bind9 restart
  ```

- Lalu cek dengan melakukan **host -t CNAME www.jarkom2020.com** atau **ping www.jarkom2020.com**. Hasilnya harus mengarah ke host dengan IP *MALANG*.


![DNS](gambar/10.png)



### 1.2.F Membuat DNS Slave

DNS Slave adalah DNS cadangan yang akan diakses jika server DNS utama mengalami kegagalan. Kita akan menjadikan server *MOJOKERTO* sebagai DNS slave dan server *MALANG* sebagai DNS masternya.

#### I. Konfigurasi Pada Server MALANG

- Edit file **/etc/bind/named.conf.local** dan sesuaikan dengan syntax berikut

  ```
  zone "jarkom2020.com" {
      type master;
      notify yes;
      also-notify { "IP MOJOKERTO"; }; // Masukan IP MOJOKERTO tanpa tanda petik
      allow-transfer { "IP MOJOKERTO"; }; // Masukan IP MOJOKERTO tanpa tanda petik
      file "/etc/bind/jarkom/jarkom2020.com";
  };
  ```

  ![DNS](gambar/11.png)



- Lakukan restart bind9

  ```
  service bind9 restart
  ```



#### II. Konfigurasi Pada Server MOJOKERTO

- Buka *MOJOKERTO* dan update package lists dengan menjalankan command:

  ```
  apt-get update
  ```

- Setalah melakukan update silahkan install aplikasi bind9 pada *MOJOKERTO* dengan perintah:

  ```
  apt-get install bind9 -y
  ```

- Kemudian buka file **/etc/bind/named.conf.local** pada MOJOKERTO dan tambahkan syntax berikut:

  ```
  zone "jarkom2020.com" {
      type slave;
      masters { "IP MALANG"; }; // Masukan IP MALANG tanpa tanda petik
      file "/var/lib/bind/jarkom2020.com";
  };
  ```

![DNS](gambar/12.png)

- Lakukan restart bind9

  ```
  service bind9 restart
  ```



#### III. Testing

- Pada server *MALANG* silahkan matikan service bind9

  ```
  service bind9 stop
  ```

- Pada client *GRESIK* pastikan pengaturan nameserver mengarah ke IP *MALANG* dan IP *MOJOKERTO*

  ![DNS](gambar/13.png)

- Lakukan ping ke jarkom2020.com pada client *GRESIK*. Jika ping berhasil maka konfigurasi DNS slave telah berhasil


![DNS](gambar/14.png)



### 1.2.G Membuat Subdomain

Subdomain adalah bagian dari sebuah nama domain induk. Subdomain umumnya mengacu ke suatu alamat fisik di sebuah situs contohnya: **jarkom2020.com** merupakan sebuah domain induk. Sedangkan **neko.jarkom2020.com** merupakan sebuah subdomain.

- Edit file **/etc/bind/jarkom/jarkom2020.com** lalu tambahkan subdomain untuk **jarkom2020.com** yang mengarah ke IP *MALANG*.

  ```
  nano /etc/bind/jarkom/jarkom2020.com
  ```

- Tambahkan konfigurasi seperti pada gambar ke dalam file **jarkom2020.com**.

![DNS](gambar/15.png)

- Restart service bind  

  ```
  service bind9 restart
  ```

- Coba ping ke subdomain dengan perintah berikut dari client *GRESIK*

  ```
  ping neko.jarkom2020.com
  
  ATAU
  
  host -t A neko.jarkom2020.com
  ```

  ![DNS](gambar/16.png)



### 1.2.H Delegasi Subdomain

Delegasi subdomain adalah pemberian wewenang atas sebuah subdomain kepada DNS baru.

#### I. Konfigurasi Pada Server *MALANG*

- Pada *MALANG*, edit file **/etc/bind/jarkom/jarkom2020.com** dan ubah menjadi seperti di bawah ini sesuai dengan pembagian IP *MALANG* kelompok masing-masing.

  ```
  nano /etc/bind/jarkom/jarkom2020.com
  ```

![DNS](gambar/17.png)

- Kemudian edit file **/etc/bind/named.conf.options** pada *MALANG*.

  ```
  nano /etc/bind/named.conf.options
  ```

- Kemudian comment **dnssec-validation auto;** dan tambahkan baris berikut pada **/etc/bind/named.conf.options**

  ```
  allow-query{any;};
  ```


![DNS](gambar/18.png)

- Kemudian edit file **/etc/bind/named.conf.local** menjadi seperti gambar di bawah:

  ```
  zone "jarkom2020.com" {
      type master;
      file "/etc/bind/jarkom/jarkom2020.com";
      allow-transfer { "IP MOJOKERTO"; }; // Masukan IP MOJOKERTO tanpa tanda petik
  };
  ```


![DNS](gambar/19.png)

- Setelah itu restart bind9

  ```
  service bind9 restart
  ```

#### II. Konfigurasi Pada Server *MOJOKERTO*

- Pada *MOJOKERTO* edit file **/etc/bind/named.conf.options**

  ```
  nano /etc/bind/named.conf.options
  ```

- Kemudian comment **dnssec-validation auto;** dan tambahkan baris berikut pada **/etc/bind/named.conf.options**

  ```
  allow-query{any;};
  ```

![DNS](gambar/20.png)

- Lalu edit file **/etc/bind/named.conf.local** menjadi seperti gambar di bawah:

![DNS](gambar/21.png)

- Kemudian buat direktori dengan nama **delegasi** 

- Copy **db.local** ke direktori pucang dan edit namanya menjadi **its.jarkom2020.com** 

  ```
  mkdir /etc/bind/delegasi
  cp /etc/bind/db.local /etc/bind/delegasi/its.jarkom2020.com
  ```

- Kemudian edit file **its.jarkom2020.com** menjadi seperti dibawah ini

![DNS](gambar/22.png)

- Restart bind9

  ```
  service bind9 restart
  ```

#### III. Testing

- Lakukan ping ke domain **its.jarkom2020.com** dan **integra.its.jarkom2020.com** dari client *GRESIK*

![DNS](gambar/23.png)



### 1.2.I DNS Forwarder

DNS Forwarder digunakan untuk mengarahkan DNS Server ke IP yang ingin dituju.

- Edit file **/etc/bind/named.conf.options** pada server *MALANG*
- Uncomment pada bagian ini

```
forwarders {
    8.8.8.8;
};
```
- Comment pada bagian ini

```
// dnssec-validation auto;
```
- Dan tambahkan

```
allow-query{any;};
```

![DNS](gambar/24.png)

- Harusnya jika nameserver pada file **/etc/resolv.conf** di client diubah menjadi IP MALANG maka akan di forward ke IP DNS google yaitu 8.8.8.8 dan bisa mendapatkan koneksi.
- Coba ping google.com pada GRESIK, kalau benar maka tetap bisa mendapatkan respon dari google

![DNS](gambar/25.png)



### 1.3 Keterangan Configurasi Zone file

1. #### Penulisan Serial

   Ditulis dengan format YYYYMMDDXX. Serial di increment setiap melakukan perubahan pada file zone.

   ```
   YYYY adalah tahun
   MM adalah bulan
   DD adalah tanggal
   XX adalah counter
   ```

   Contoh:

   ![DNS](gambar/7.png)

2. #### Penggunaan Titik

   ![DNS](gambar/17.png)

   Pada salah satu contoh di atas, dapat kita amati pada kolom keempat terdapat record yang menggunakan titik pada akhir kata dan ada yang tidak. Penggunaan titik berfungsi sebagai penentu FQDN (Fully-Qualified Domain Name) suatu domain.

   Contohnya jika "**jarkom2020.com.**" di akhiri dengan titik maka akan dianggap sebagai FQDN dan akan dibaca sebagai "**jarkom2020.com**" , sedangkan ns1 di atas tidak menggunakan titik sehingga dia tidak terbaca sebagai FQDN. Maka ns1 akan di tambahkan di depan terhadap nilai $ORIGIN sehinga ns1 akan terbaca sebagai "**ns1.jarkom2020.com**" . Nilai $ORIGIN diambil dari penamaan zone yang terdapat pada  */etc/bind/named.conf.local*.

3. #### Penulisan Name Server (NS) record

   Salah satu aturan penulisan NS record adalah dia harus menuju A record., bukan CNAME. 
