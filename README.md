# Jarkom_Modul2_Lapres_C01
Praktikum Modul 2 Jaringan Komputer 2020

## Nama Anggota Kelompok :

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

(15) Bibah meminta kamu membuat web http:// naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” supaya
aman dan tidak sembarang orang bisa mengaksesnya. Saat Bibah mengunjungi IP PROBOLINGGO, yang muncul bukan web utama http:// semeruyyy.pw melainkan laman default Apache yang bertuliskan “It works!”. 

(16) Karena dirasa kurang profesional, maka setiap Bibah mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke http:// semeruyyy.pw. (17) Karena pengunjung pada /var/www/ penanjakan.semeruyyy.pw/public/images sangat banyak maka semua request gambar yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.

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

Kemudian kita buat file proxy.sh yang berisi id dan password yang sudah didapat, kemudian buat pada setiap UML. File proxy.sh dapat dijalankan dengan menggunakan command `source proxy.sh`.

Kemudian lakukan update pada semua UML dengan mengetikkan apt-get update.
