# Jarkom_Modul2_Lapres_C01
Praktikum Modul 2 Jaringan Komputer 2020

## NAMA ANGGOTA KELOMPOK :

**1. Devi Hainun Pasya (05111840000014)**

**2. Kevin Christian Hadinata (05111840000066)**

## PENJELASAN SOAL
Semeru adalah salah satu gunung yang terkenal di Jawa Timur. Bibah adalah salah satu juru kunci Semeru. Bibah ingin menyebarkan keindahan Semeru pada dunia sehingga dia membeli 3 buah server yang berada di MALANG, MOJOKERTO dan PROBOLINGGO. Server MALANG akan digunakan sebagai DNS Server Master, MOJOKERTO akan digunakan sebagai DNS Server Slave dan PROBOLINGGO akan digunakan sebagai Web Server. Selain 3 server terdapat 2 klien yang digunakan untuk testing oleh Bibah yaitu GRESIK dan SIDOARJO. Untuk menyambungkan semua jaringan tersebut Bibah memberi router di SURABAYA.

Kalian diminta untuk membuat sebuah website utama dengan :

(1) alamat http://semeruyyy.pw yang memiliki 

(2) alias http://www.semeruyyy.pw, dan 

(3) subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO serta dibuatkan 

(4) reverse domain untuk domain utama. Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan

(5) DNS Server Slave pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website. Selain website utama Bibah juga meminta dibuatkan 

(6) subdomain dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO. Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan 

(7) subdomain dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO.

Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server. 

(8) Domain http://semeruyyy.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw. Awalnya web dapat diakses menggunakan alamat http://semeruyyy.pw/index.php/home. Karena dirasa alamat urlnya kurang bagus, maka 

(9) diaktifkan mod rewrite agar urlnya menjadi http://semeruyyy.pw/home.

(10) Web http://penanjakan.semeruyyy.pw akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur
folder sebagai berikut: 

/var/www/ penanjakan.semeruyyy.pw
/var/www/ penanjakan.semeruyyy.pw/public/javascripts
/var/www/ penanjakan.semeruyyy.pw/public/css
/var/www/ penanjakan.semeruyyy.pw/public/images
/var/www/ penanjakan.semeruyyy.pw/errors

(11) Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan. 

(12) Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache. 

(13) Untuk mengakses file assets javascript awalnya harus menggunakan url http://penanjakan.semeruyyy.pw/public/javascripts. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi http://penanjakan.semeruyyy.pw/js. Untuk web http:// gunung.semeruyyy.pw belum dapat dikonfigurasi pada web server karena
menunggu pengerjaan website selesai. 

(14) sedangkan web http:// naik.gunung.semeruyyy.pw sudah bisa diakses hanya dengan menggunakan port 8888. DocumentRoot web berada pada /var/www/ naik.gunung.semeruyyy.pw.Dikarenakan web http:// naik.gunung.semeruyyy.pw bersifat private 

(15) Bibah meminta kamu membuat web http:// naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” supaya aman dan tidak sembarang orang bisa mengaksesnya. Saat Bibah mengunjungi IP PROBOLINGGO, yang muncul bukan web utama http:// semeruyyy.pw melainkan laman default Apache yang bertuliskan “It works!”. 

(16) Karena dirasa kurang profesional, maka setiap Bibah mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke http:// semeruyyy.pw. 

(17) Karena pengunjung pada /var/www/ penanjakan.semeruyyy.pw/public/images sangat banyak maka semua request gambar yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.

## PEMBAHASAN SOAL
Yang pertama dilakukan sebelum memulai konfigurasi adalah membuat topologi jaringan terlebih dahulu. Pertama kita jalankan Xming server, setelah Xming berjalan kemudian kita jalankan PuTTY dan masukkan IP kelas Jarkom C = 10.151.36.203

Kemudian pada category Connection >> SSH >> X11, centang kolom "Enable X11 forwarding".

Kemudian kia jalankan dengan meng-click Open. Setelah dijalankan, akan terbuka window baru. Kemudian login sebagai kelompok masing-masing menggunakan id kelompok dan password.

Setelah itu atur topologi jaringan dengan syntax berikut. Karena tertera pada soal bahwa default memory adalah 96M kecuali untuk server, maka memory setiap Kota perlu diatur, sebagai berikut :
```

```

Kemudian kita jalankan file topologi yang telah dibuat dan diatur dengan menggunakan `bash topologi.sh`. 

Setelah semua window UML muncul, login ke setiap UML dengan id "root" dan password "praktikum". Kemudian pada UML Router, yaitu SURABAYA, dilakukan setting `sysctl` dengan menggunakan command `nano /etc/sysctl.conf`. Kemudian uncomment baris `net.ipv4.ip_forward=1`.

Untuk mengaktifkan perubahan, digunakan command `sysctl -p`.

Kemudian kita lakukan setting IP untuk setiap UML dengan menggunakan command `nano /etc/network/interfaces`. Kemudian dimasukkan konfigurasi IP sebagai berikut :
```
SURABAYA (ROUTER)
```
```
MALANG (DNS SERVER MASTER)
```
```
MOJOKERTO (DNS SERVER SLAVE)
```
```
PROBOLINGGO (WEB SERVER)
```
```
GRESIK (CLIENT)
```
```
SIDOARJO (CLIENT)
```

Kemudian restart network dengan menggunakan command `service networking restart` pada setiap UML.

Kemudian jalankan iptables pada Router yaitu SURABAYA dengan menggunakan command `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16`.

Untuk memastikan konfigurasi telah dibuat dnegan benar, kita coba dengan `ping google.com`.

Kemudian kita buat file export.sh yang berisi id dan password proxy yang sudah didapat pada setiap UML. File proxy dapat dijalankan dengan menggunakan command `source export.sh`.

Kemudian lakukan update pada semua UML dengan mengetikkan `apt-get update`.

### Soal No. 1
(1) membuat website utama dengan alamat http://semeruyyy.pw

Yang pertama dilakukan adalah instalasi bind pada UML Malang. Instalasi dilakukan dengan menjalankan command `apt-get install bind9 -y`. Setelah bind terinstal, dilanjutkan dengan pembuatan domain. Ketikkan command `nano /etc/bind/named.conf.local` pada UML Malang. Kemudian isikan konfigurasi domain `semeruc01.pw` dengan syntax berikut :
```
zone "semeruc01.pw" {
	  type master;
	  file "/etc/bind/jarkom/semeruc01.pw";
};
```

***foto***

Kemudian buat folder jarkom dalam folder `etc/bind` dengan menggunakan command `mkdir /etc/bind/jarkom`. Selanjutnya copy-kan file `db.local` pada path `/etc/bind` ke dalam folder jarkom yang baru saja dibuat dan ubah namanya menjadi `semeruc01.pw` dengan menggunakan command berikut :
```
cp /etc/bind/db.local /etc/bind/jarkom/semeruc01.pw
```
Kemudian ubah konfigurasi pada file semeruc01.pw dengan menambahkan IP PROBOLINGGO sebagai berikut :

***foto***

Restart bind9 dengan command `service bind9 restart`.

**HASIL :**

***foto***

### Soal No. 2
(2) menambah alias http://www.semeruyyy.pw

Buka file `/etc/bind/jarkom/semeruc01.pw` pada UML Malang kemudian tambahkan konfigurasi sebagai berikut :

***foto***

Restart bind9 dengan command `service bind9 restart`.

**HASIL :**

***foto***

### Soal No. 3
(3) buat subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO

Yang pertama dilakukan adalah edit file `/etc/bind/jarkom/semeruc01.pw` lalu tambahkan subdomain untuk `semeruc01.pw` yang mengarah ke IP PROBOLINGGO dengan command `nano /etc/bind/jarkom/semeruc01.pw`. Kemudian tambahkan konfigurasi sebagai berikut :

***foto***

Restart bind9 dengan command `service bind9 restart`.

**HASIL :**

***foto***

### Soal No. 4
(4) buatkan reverse domain untuk domain utama.

Pertama-tama edit file `/etc/bind/named.conf.local` pada UML MALANG dengan menambahkan konfigurasi sebagai berikut :
```
zone "77.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/77.151.10.in-addr.arpa";
};
```
Kemudian copy-kan file `db.local` pada path `/etc/bind` ke dalam folder jarkom yang baru saja dibuat dan ubah namanya menjadi `77.151.10.in-addr.arpa` dengan menggunakan command berikut :
```
cp /etc/bind/db.local /etc/bind/jarkom/77.151.10.in-addr.arpa
```
Kemudian edit file `/etc/bind/jarkom/77.151.10.in-addr.arpa` menjadi seperti gambar berikut :

***foto***

Restart bind9 dengan command `service bind9 restart`.

**HASIL :**

***foto***

### Soal No. 5
(5) buatkan DNS Server Slave pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website.

Pada UML MALANG, ubah file `/etc/bind/named.conf.local` dengan menambahkan syntax berikut :
```
zone "semeruc01.pw" {
    type master;
    notify yes;
    also-notify { 10.151.77.19; }; // IP MOJOKERTO
    allow-transfer { 10.151.77.19; }; // IP MOJOKERTO
    file "/etc/bind/jarkom/semeruc01.pw";
};
```
***foto***

Restart bind9 MALANG dengan command `service bind9 restart`.

Kemudian pada UML MOJOKERTO, ubah file `/etc/bind/named.conf.local` dengan menambahkan syntax berikut :
```
zone "semeruc01.pw" {
    type master;
    master { 10.151.77.18; }; // IP MALANG
    file "/etc/bind/jarkom/semeruc01.pw";
```
***foto***

Restart bind9 MOJOKERTO dengan command `service bind9 restart`.

**HASIL :**

***foto***

### Soal No.6 
(6) buatkan subdomain dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO.

Pada UML MALANG edit file `/etc/bind/jarkom/semeruc01.pw` sebagai berikut :

***foto***

Kemudian edit file `/etc/bind/named.conf.options` pada UML MALANG dan tambahkan `allow-query {any;}`.

***foto***

Kemudian edit file `/etc/bind/jarkom/semeruc01.pw` dengan menambahkan syntax berikut :
```
zone "semeruc01.pw" {
    type master;
    notify yes;
    also-notify { 10.151.77.19; }; // IP MOJOKERTO
    allow-transfer { 10.151.77.19; }; // IP MOJOKERTO
    file "/etc/bind/jarkom/semeruc01.pw";
};
```
***foto***

Pada UML MOJOKERTO, edit file `/etc/bind/named.conf.options` dengan menambahkan `allow-query {any;}`.

***foto***

Kemudian buat direktori delegasi dengan menggunakan command `mkdir /etc/bind/delegasi`. Kemudian copy `db.local` dengan command sebagai berikut :
```
cp /etc/bind/db.local /etc/bind/delegasi/gunung.semeruc01.pw
```
Restart bind9 MOJOKERTO dengan command `service bind9 restart`.

**HASIL :**

***foto***

### Soal No.7
(7) buat subdomain dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO.

Pada UML MALANG, edit file `/etc/bind/jarkom/semeruc01.pw` dengan menambahkan konfigurasi sebagai berikut :

***foto***

Kemudian pada UML MOJOKERTO, edit file `/etc/bind/delegasi/gunung.semeruc01.pw` dengan menambahkan konfigurasi sebagai berikut :

***foto***

Kemudian edit file `/etc/bind/named.conf.local` dengan menambahkan konfigurasi sebagai berikut :
```
zone "gunung.semeruc01.pw"{
       type master;
       file "/etc/bind/delegasi/gunung.semeruc01.pw";
       allow-transfer { any; };
};
```
***foto***

Restart bind9 MOJOKERTO dengan command `service bind9 restart`.

**HASIL :**

***foto***

Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server. 

### Soal No.8
(8) Domain http://semeruyyy.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw.

Pertama-tama, pada UML PROBOLINGGO, install apache dengan command `apt-get install apache2` dan install php dengan command `apt-get install php7`. 

Kemudian download file pendukung yang tertera pada file soal dengan command `wget 10.151.36.202/semeru.pw.zip` pada directory `/var/www/semeruc01.pw`. Kemudian unzip file dengan command `unzip semeru.pw.zip`.

Kemudian edit file default yang terdapat di folder sites-available dengan command `nano /etc/apache2/sites-available/000-default.conf` sebagai berikut :

***foto***

Kemudian pindah ke folder `/etc/apache2/sites-available/` dan copy file `000-default.conf` ke file `semeruc01.pw.conf` dengan menggunakan command `cp 000-default.conf semeruc01.pw.conf`. Tambahkan konfigurasi berikut ke dalam file `semeruc01.pw.conf` :
```
ServerName semeruc01.pw
ServerAlias www.semeruc01.pw
```
Kemudian aktifkan konfigurasi yang telah dibuat dengan command `a2ensite semeruc02.pw`.

Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.9
(9) aktifkan mod rewrite agar urlnya menjadi http://semeruyyy.pw/home.

Pertama aktifkan dahulu module rewrite dengan menggunakan commad `a2enmode rewrite` dan Restart apache dengan command `service apache2 restart`.

Kemudian buat file `.htaccess` pada directory `/var/www/semeruc01.pw` yang diisi dengan konfigurasi sebagai berikut :
```
 RewriteEngine On
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^([^\.]+)$ $1.php [NC,L]
```
***foto***

Selanjutnya pada folder sites-available, buka file `semeruc01.pw.conf` dan tambahkan konfigurasi berikut :
```
<Deirectory /var/www/semeruc01.pw>
	  Options +FollowSymLinks -Multiviews
	  AllowOverride ALL
</Directory>
```

Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.10
(10) Web http://penanjakan.semeruyyy.pw akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur
folder sebagai berikut: 

/var/www/ penanjakan.semeruyyy.pw
/var/www/ penanjakan.semeruyyy.pw/public/javascripts
/var/www/ penanjakan.semeruyyy.pw/public/css
/var/www/ penanjakan.semeruyyy.pw/public/images
/var/www/ penanjakan.semeruyyy.pw/errors

Pada UML PROBOLINGGO download file pendukung yang tertera pada file soal dengan command `wget 10.151.36.202/penanjakan.semeru.pw.zip` pada directory `/var/www/penanjakan.semeruc01.pw`. Kemudian unzip file dengan command `unzip penanjakan.semeru.pw.zip`.

Ubah nama file `penanjakan.semeru.pw` menjadi `penanjakan.semeruc01.pw` dengan menggunakan command berikut :
```
mv penanjakan.semeru.pw penanjakan.semeruc01.pw
```

Kemudian pindah ke folder `/etc/apache2/sites-available/` dan copy file `000-default.conf` ke file `penanjakan.semeruc01.pw.conf` dengan menggunakan command `cp 000-default.conf penanjakan.semeruc01.pw.conf`. Tambahkan konfigurasi berikut ke dalam file `penanjakan.semeruc01.pw.conf` :
```
ServerName penanjakan.semeruc01.pw
ServerAlias www.penanjakan.semeruc01.pw
```
Kemudian ubah folder document root menjadi `/var/www/penanjakan.semeruc01.pw`.

***foto***

Kemudian aktifkan konfigurasi yang telah dibuat dengan command `a2ensite penanjakan.semeruc02.pw`.

Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.11
(11) Pada folder `/public` dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.

Pada UML PROBOLINGGO, buka file `/etc/apache2/sites-available/penanjakan.semeruc01.pw.conf` dan tambahkan konfigurasi sebagai berikut untuk mengubah access dari masing-masing folder :
```
<Directory /var/www/penanjakan.semeruc01.pw>
     Options +Indexes
 </Directory>
 <DirectoryMatch ^/var/www/penanjakan.semeruc01.pw/public/[a-z]>
     Options -Indexes
 </DirectoryMatch>
```
Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.12
(12) Untuk mengatasi HTTP Error code 404, disediakan file `404.html` pada folder `/errors` untuk mengganti error default 404 dari Apache.

Pada UML PROBOLINGGO, buka file `/etc/apache2/sites-available/penanjakan.semeruc01.pw.conf` dan tambahkan konfigurasi sebagai berikut :
```
ErrorDocument 404 /errors/404.html
```
Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.13
(13) Untuk mengakses file assets javascript awalnya harus menggunakan url http://penanjakan.semeruyyy.pw/public/javascripts. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi http://penanjakan.semeruyyy.pw/js. 

Pada UML PROBOLINGGO, buka file `/etc/apache2/sites-available/penanjakan.semeruc01.pw.conf` dan tambahkan konfigurasi sebagai berikut :
```
Alias "/js" "/var/www/penanjakan.semeruc01.pw/public/javascripts"
```
Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.14
(14) Untuk web http:// gunung.semeruyyy.pw belum dapat dikonfigurasi pada web server karena menunggu pengerjaan website selesai. sedangkan web http:// naik.gunung.semeruyyy.pw sudah bisa diakses hanya dengan menggunakan port 8888.

Pada UML PROBOLINGGO download file pendukung yang tertera pada file soal dengan command `wget 10.151.36.202/naik.gunung.semeru.pw.zip` pada directory `/var/www/naik.gunung.semeruc01.pw`. Kemudian unzip file dengan command `unzip naik.gunung.semeru.pw.zip`.

Ubah nama file `naik.gunung.semeru.pw` menjadi `naik.gunung.semeruc01.pw` dengan menggunakan command berikut :
```
mv naik.gunung.semeru.pw naik.gunung.semeruc01.pw
```

Kemudian pindah ke folder `/etc/apache2/sites-available/` dan copy file `000-default.conf` ke file `naik.gunung.semeruc01.pw.conf` dengan menggunakan command `cp 000-default.conf naik.gunung.semeruc01.pw.conf`. Tambahkan konfigurasi berikut ke dalam file `naik.gunung.semeruc01.pw.conf` :
```
ServerName naik.gunung.semeruc01.pw
ServerAlias www.naik.gunung.semeruc01.pw
```
Kemudian ubah folder document root menjadi `/var/www/naik.gunung.semeruc01.pw` dan virtual host menjadi 8888.

***foto***

Kemudian pada file `/etc/apache2/ports.conf`, tambahkan `Listen 8888`.

***foto***

Kemudian aktifkan konfigurasi yang telah dibuat dengan command `a2ensite naik.gunung.semeruc02.pw`.

Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.15
(15) Bibah meminta kamu membuat web http:// naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” supaya aman dan tidak sembarang orang bisa mengaksesnya. 

**HASIL :**

***foto***

### Soal No.16
(16) Karena dirasa kurang profesional, maka setiap Bibah mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke http:// semeruyyy.pw.

Pada UML PROBOLINGGO, buka file `/etc/apache2/sites-available/000-default.conf`. Kemudian edite folder document root menjadi `/var/www/semeruc01.pw`.

***foto***

Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***

### Soal No.16
(17) Karena pengunjung pada /var/www/ penanjakan.semeruyyy.pw/public/images sangat banyak maka semua request gambar yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.

Restart apache dengan command `service apache2 restart`.

**HASIL :**

***foto***
