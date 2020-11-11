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

### 1. Membuat DNS

- Membuat topologi berdasarkan foto pada soal.

### 2. Instalasi bind

- Buka _MALANG_ dan update package lists dengan menjalankan command:

  ```
  apt-get update
  ```

- Setalah melakukan update, install aplikasi bind9 pada _MALANG_ dengan perintah:

  ```
  apt-get install bind9 -y
  ```

### 3. Pembuatan Domain

Pada Soal Shift 2 ini, Bibah ingin membuat website utama dengan alamat **semerud04.pw**.

- Lakukan perintah pada _MALANG_. Isikan seperti berikut:

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

- Kemudian buka file **semerud04.pw** dan edit seperti gambar berikut dengan IP _MALANG_ , yaitu `10.151.79.42`

  ```
  nano /etc/bind/jarkom/jarkom2020.com
  ```

![konfig jarkom2020](gambar/3.png)

- Restart bind9 dengan perintah

  ```
  service bind9 restart
  ```

### 4. Setting nameserver pada client

Domain yang kita buat tidak akan langsung dikenali oleh client oleh sebab itu kita harus merubah settingan nameserver yang ada pada client kita.

- Pada client _GRESIK_ dan _SIDOARJO_ arahkan nameserver menuju IP _MALANG_ `10.151.79.42` dengan mengedit file _resolv.conf_ dengan mengetikkan perintah

  ```
  nano /etc/resolv.conf
  ```

![ping](gambar/4.png)

- Untuk mencoba koneksi DNS, lakukan ping domain **semerud04.pw** dengan melakukan perintah berikut pada client _GRESIK_ dan _SIDOARJO_

  ```
  ping jarkom2020.com
  ```

![ping](gambar/5.png)

### 5. Menambah alias menggunakan Record CNAME

- Buka file **semerud04.pw** pada server _MALANG_ dan tambahkan konfigurasi seperti pada gambar berikut:

![DNS](gambar/9.png)

- Kemudian restart bind9 dengan perintah

  ```
  service bind9 restart
  ```

- Lalu cek dengan melakukan **ping www.semerud04.pw**. Hasilnya harus mengarah ke host dengan IP _MALANG_ `10.151.79.42`.

![DNS](gambar/10.png)

### 6. Membuat Subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO `10.151.79.44`

- Edit file **/etc/bind/jarkom/semerud04.pw** lalu tambahkan subdomain untuk **semerud04.pw** yang mengarah ke IP _MALANG_.

  ```
  nano /etc/bind/jarkom/semerud04.pw
  ```

- Tambahkan konfigurasi seperti pada gambar ke dalam file **semerud04.pw**.

![DNS](gambar/15.png)

- Restart service bind

  ```
  service bind9 restart
  ```

- Coba ping ke subdomain dengan perintah berikut dari client _GRESIK_

  ```
  ping penanjakan.semerud04.pw

  ```

  ![DNS](gambar/16.png)

### 7. Reverse domain untuk domain utama

- Edit file **/etc/bind/named.conf.local** pada _MALANG_

  ```
  nano /etc/bind/named.conf.local
  ```

- Lalu tambahkan konfigurasi berikut ke dalam file **named.conf.local**

  ```
  zone "79.151.10.in-addr.arpa" {
      type master;
      file "/etc/bind/jarkom/79.151.10.in-addr.arpa";
  };
  ```

![](gambar/6.png)

- Copykan file **db.local** pada path **/etc/bind** ke dalam folder **jarkom** yang baru saja dibuat dan ubah namanya menjadi **79.151.10.in-addr.arpa**

  ```
  cp /etc/bind/db.local /etc/bind/jarkom/79.151.10.in-addr.arpa
  ```

- Edit file **79.151.10.in-addr.arpa** menjadi seperti gambar di bawah ini

![konfig](gambar/7.png)

- Kemudian restart bind9 dengan perintah

  ```
  service bind9 restart
  ```

- Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client _GRESIK_

  ```
  // Install package dnsutils
  // Pastikan nameserver telah dikembalikan ke settingan awal
  apt-get update
  apt-get install dnsutils

  //Kembalikan nameserver agar tersambung dengan MALANG
  host -t PTR "IP MALANG"
  ```

![host](gambar/8.png)

### 8. Membuat DNS Server Slave pada MOJOKERTO

#### 8.1. Konfigurasi Pada Server MALANG

- Edit file **/etc/bind/named.conf.local** dan sesuaikan dengan syntax berikut

  ```
  zone "semerud04.pw" {
      type master;
      notify yes;
      also-notify { 10.151.79.43; }; // Masukan IP MOJOKERTO tanpa tanda petik
      allow-transfer { 10.151.79.43; }; // Masukan IP MOJOKERTO tanpa tanda petik
      file "/etc/bind/jarkom/semerud04.pw";
  };
  ```

  ![DNS](gambar/11.png)

- Lakukan restart bind9

  ```
  service bind9 restart
  ```

#### 8.2. Konfigurasi Pada Server MOJOKERTO

- Buka _MOJOKERTO_ dan update package lists dengan menjalankan command:

  ```
  apt-get update
  ```

- Setalah melakukan update silahkan install aplikasi bind9 pada _MOJOKERTO_ dengan perintah:

  ```
  apt-get install bind9 -y
  ```

- Kemudian buka file **/etc/bind/named.conf.local** pada MOJOKERTO dan tambahkan syntax berikut:

  ```
  zone "semerud04.pw" {
      type slave;
      masters { 10.151.79.42; }; // Masukan IP MALANG tanpa tanda petik
      file "/var/lib/bind/semerud04.pw";
  };
  ```

![DNS](gambar/12.png)

- Lakukan restart bind9

  ```
  service bind9 restart
  ```

#### 8.3. Testing

- Pada server _MALANG_ silahkan matikan service bind9

  ```
  service bind9 stop
  ```

- Pada client _GRESIK_ pastikan pengaturan nameserver mengarah ke IP _MALANG_ `10.151.79.42` dan IP _MOJOKERTO_ `10.151.79.43`

  ![DNS](gambar/13.png)

- Lakukan ping ke semerud04.pw pada client _GRESIK_. Jika ping berhasil maka konfigurasi DNS slave telah berhasil

![DNS](gambar/14.png)

### 1.2.H Delegasi Subdomain

Delegasi subdomain adalah pemberian wewenang atas sebuah subdomain kepada DNS baru.

#### I. Konfigurasi Pada Server _MALANG_

- Pada _MALANG_, edit file **/etc/bind/jarkom/jarkom2020.com** dan ubah menjadi seperti di bawah ini sesuai dengan pembagian IP _MALANG_ kelompok masing-masing.

  ```
  nano /etc/bind/jarkom/jarkom2020.com
  ```

![DNS](gambar/17.png)

- Kemudian edit file **/etc/bind/named.conf.options** pada _MALANG_.

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

#### II. Konfigurasi Pada Server _MOJOKERTO_

- Pada _MOJOKERTO_ edit file **/etc/bind/named.conf.options**

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

- Lakukan ping ke domain **its.jarkom2020.com** dan **integra.its.jarkom2020.com** dari client _GRESIK_

![DNS](gambar/23.png)

### 1.2.I DNS Forwarder

DNS Forwarder digunakan untuk mengarahkan DNS Server ke IP yang ingin dituju.

- Edit file **/etc/bind/named.conf.options** pada server _MALANG_
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

   Contohnya jika "**jarkom2020.com.**" di akhiri dengan titik maka akan dianggap sebagai FQDN dan akan dibaca sebagai "**jarkom2020.com**" , sedangkan ns1 di atas tidak menggunakan titik sehingga dia tidak terbaca sebagai FQDN. Maka ns1 akan di tambahkan di depan terhadap nilai $ORIGIN sehinga ns1 akan terbaca sebagai "**ns1.jarkom2020.com**" . Nilai $ORIGIN diambil dari penamaan zone yang terdapat pada _/etc/bind/named.conf.local_.

3. #### Penulisan Name Server (NS) record

   Salah satu aturan penulisan NS record adalah dia harus menuju A record., bukan CNAME.

4. Langkah awalnya dengan membuat direktori pada /var/www, dengan perintah `mkdir /var/www/semerud04.pw`. Setelah itu buatlah file config untuk file semerud04.pw, yaitu dengan menjalankan perintah `cp /etc/apache2/site-available/default /etc/apache2/sites-available/semerud04.pw`. Lalu buka file tersebut dengan `nano /etc/apache2/sites-available/semerud04.pw` dan ubah file menjadi seperti gambar dibawah ini :

![semerud04.pw](/img/no-8.png)

Setelah itu, save dan exit. Lalu aktifkan file tersebut dengan perintah `a2ensite semerud04.pw` dan berikutnya perintah `service apache2 restart`.

Lalu jalankan perintah berikut ini :

```bash
cd /var/www/semerud04.pw
wget 10.151.36.202/semeru.pw.zip
unzip semeru.pw.zip
cd semeru.pw
mv * ../
rm -r semeru.pw semeru.pw.zip
```

Struktur folder semerud04.pw akan terlihat sebagai berikut:

![semerud04.pw](/img/no-8-part-2.png)

Akses website http://semerud04.pw/index.php/home, sehingga akan menampilkan :

![semerud04.pw](/img/no-8-part-3.png)

9. Buatlah file .htaccess pada folder /var/www/semerud04.pw. Dan isikan file tersebut seperti pada gambar berikut:

![semerud04.pw](/img/no-9.png)

```
Keterangan:
RewriteEngine On -> mengaktifkan modul rewrite pada apache
RewriteCond %{REQUEST_FILENAME} !-d -> aturan tidak akan jalan ketika yang diakses adalah directory
RewriteRule &([^\.]+)$ index.php/$1 [NC,L] -> segala url yang berawalan /index.php/nama_page akan bisa diakses menjadi /nama_page
```

10. Langkah awalnya dengan membuat direktori pada /var/www, dengan perintah `mkdir /var/www/penanjakan.semerud04.pw`. Setelah itu buatlah file config untuk file semerud04.pw, yaitu dengan menjalankan perintah `cp /etc/apache2/site-available/default /etc/apache2/sites-available/penanjakan.semerud04.pw`. Lalu buka file tersebut dengan `nano /etc/apache2/sites-available/penanjakan.semerud04.pw` dan ubah file menjadi seperti gambar dibawah ini :

![semerud04.pw](/img/no-10.png)

Setelah itu, save dan exit. Lalu aktifkan file tersebut dengan perintah `a2ensite semerud04.pw` dan berikutnya perintah `service apache2 restart`.

Lalu jalankan perintah berikut ini :

```bash
cd /var/www/penanjakan.semerud04.pw
wget 10.151.36.202/penanjakan.semeru.pw.zip
unzip penanjakan.semeru.pw.zip
cd penanjakan.semeru.pw
mv * ../
rm -r penanjakan.semeru.pw penanjakan.semeru.pw.zip
```

Struktur folder semerud04.pw akan terlihat sebagai berikut:

![semerud04.pw](/img/no-10-part-2.png)

Akses website http://penanjakan.semerud04.pw, sehingga akan menampilkan :

![semerud04.pw](/img/no-10-part-3.png)

11. Ketikkan `nano /etc/apache2/sites-available/penanjakan.semerud04.pw` dan ubah file menjadi seperti berikut ini:

![semerud04.pw](/img/no-11.png)

Restart dengan `service apache2 restart`. Lalu akses melalu http://penanjakan.semerud04.pw/public, sehingga akan menampilkan

![semerud04.pw](/img/no-11-part-2.png)

Lalu akses lagi salah satu folder seperti http://penanjakan.semerud04.pw/public/css dan akan menampilkan

![semerud04.pw](/img/no-11-part-3.png)

12. Untuk menampilkan custom file error, buat file .htaccess pada folder /var/www/penanjakan.semerud04.pw, lalu isi file seperti berikut ini:

![semerud04.pw](/img/no-12.png)

Lalu simpan dan buka url http://penanjakan.semerud04.pw/public/css/imagess sehingga akan menampilkan

![semerud04.pw](/img/no-12-part-2.png)

13. Jalankan perintah `nano /etc/apache2/sites-available/penanjakan.semerud04.pw`. Lalu ubah isi file pada bagian Alias /js seperti pada gambar.

![semerud04.pw](/img/no-13.png)

Lalu restart dengan `service apache2 restart`. Akses url http://penanjakan.semerud04.pw/js/index.js sehingga akan menampilkan

![semerud04.pw](/img/no-13-part-2.png)

14. Jalankan perintah `nano /etc/apache2/ports.conf`. Lalu ubah file seperti berikut

![semerud04.pw](/img/no-14.png)

Setelah itu membuat direktori pada /var/www, dengan perintah `mkdir /var/www/naik.gunung.semerud04.pw`. Setelah itu buatlah file config untuk file semerud04.pw, yaitu dengan menjalankan perintah `cp /etc/apache2/site-available/default /etc/apache2/sites-available/naik.gunung.semerud04.pw`. Lalu buka file tersebut dengan `nano /etc/apache2/sites-available/naik.gunung.semerud04.pw` dan ubah file menjadi seperti gambar dibawah ini :

![semerud04.pw](/img/no-14-part-2.png)

Setelah itu, save dan exit. Lalu aktifkan file tersebut dengan perintah `a2ensite naik.gunung.semerud04.pw` dan berikutnya perintah `service apache2 restart`.

Lalu jalankan perintah berikut ini :

```bash
cd /var/www/naik.gunung.semerud04.pw
wget 10.151.36.202/naik.gunung.semeru.pw.zip
unzip naik.gunung.semeru.pw.zip
cd semeru
mv * ../
rm -r semeru naik.gunung.semeru.pw.zip
```

Struktur folder naik.gunung.semerud04.pw akan terlihat sebagai berikut:

![semerud04.pw](/img/no-14-part-3.png)

Akses website http://naik.penanjakan.semerud04.pw:8888, sehingga akan menampilkan :

![semerud04.pw](/img/no-14-part-4.png)

15. Jalankan perintah `htpasswd -c /etc/apache2/htpasswd semeru` dan isi password dengan `kuynaikgunung`. Lalu ubah file menjadi seperti berikut ini :

![semerud04.pw](/img/no-15.png)

Lalu restart dengan `service apache2 restart`. Akses website http://naik.penanjakan.semerud04.pw:8888, sehingga akan menampilkan :

![semerud04.pw](/img/no-15-part-2.png)

Dan ketika berhasil masuk akan menjadi seperti ini

![semerud04.pw](/img/no-15-part-3.png)

16. Jalankan `nano /etc/apache2/sites-available/default` dan ubah file menjadi berikut :

![semerud04.pw](/img/no-16.png)

Restart dengan `service apache2 restart` dan akses http://10.151.79.44/ sehingga akan menampilkan

![semerud04.pw](/img/no-16-part-2.png)

17. Buka file .htaccess pada /var/www/penanjakan.semerud04.pw dan ubah menjadi seperti berikut isi filenya :

![semerud04.pw](/img/no-17.png)

```
Keterangan:
RewriteEngine On -> mengaktifkan modul rewrite pada apache
RewriteCond %{REQUEST_FILENAME} -f -> Mengecek apakah request image yang kita buat memang ada di folder /public/images
RewriteCond %{REQUEST_URI} ! /public/images/semeru.jpg [NC] -> mengecek apakah file yang sedang kita buka merupakan file semeru.jpg
RewriteRule ^public/images/(.*)semeru(.*).jpg$ /public/images/$1semeru.jpg[L,R] -> Jika membuka file yang mengandung substring semeru pada folder /public/images, akan di redirect ke /public/images/semeru.jpg selama kedua kondisi diatas terpenuh
```
