---
title:  "Membuat instalasi Wordpress lokal dan <em>remote</em> serta sinkronisasinya"
date:   2017-12-23 00:00:00 +0700
short_desc : '<p>Cadangan<br>energi</p>' 
tags:
- administration
- wordpress
excerpt: This is an excerpt
header_image: www/wordpress/blogging-blur-business-261662.jpg
---
### Pengantar ~~Tidur~~
Situasinya sebagai berikut: Anda punya laptop dan juga punya *server* di jagat maya, lalu Anda ingin membuat instalasi Wordpress di lokal dan di server tersebut, juga ingin melakukan sinkronisasinya. (Atau cuma saya saja yang mau begini, tidak ada yang tahu).

Kelebihannya kalau begini, Anda bisa *ngeblog* sepuas-puasnya dengan kuota 0 Byte. Nanti kalau *blog* sudah dalam keadaan yang Anda inginkan, tinggal jalankan *script* sinkronisasi yang dijelaskan di sini. Kuota ke internet yang Anda pakai untuk mengirim data jadi lebih efisien karena proses penyuntingan yang berkali-kali dilakukan secara lokal. Anda pun bisa memanfaatkan seluruh fasilitas Wordpress secara lengkap.

Walaupun agak salah kaprah, saya menganggap ini juga sebagai metode *backup*. Kapan pun *server* di jagat maya lenyap, saya punya perubahan terkini di laptop saya. Kalau laptop dijambret, (amit-amit!) masih ada salinan di jagat maya. Kalau dua-duanya terjadi? *Apes bener.* Jalankan metode sinkronisasi yang disesuaikan agar menyimpan berkas sql dan berkas file Wordpress ke dalam *tarball* lalu dikirim lewat email ke mailbox yang menurut Anda ukurannya besar. <sup>Next homework.</sup>

## Asumsi
* Fedora (laptop) dan CentOS server.
* Nginx
* PHP-FPM
* MariaDB
* php-mysqli *extension* untuk PHP `# dnf install php-mysqli`

## Langkah
Perangkat lunak di bagian Asumsi sudah terpasang, terkonfigurasi.

### 1. Membuat Direktori, Pengaturan Hak Akses, Pengaturan SELinux
Siapkan direktori webapp sebagai penampung semua berkas web. Lalu di bawah webapp, buat folder per user Linux menurut pool PHP-FPM yang disiapkan. Di bawah folder itu, ada tiga folder, yaitu `files`, `logs`, `php-fpm-session`.

Direktorinya akan jadi seperti ini: <sup>(pertimbangkan keluarkan ini ke artikel tersendiri)</sup>
```
/var/webapp                       -> all webapp
/var/webapp/ferdi                 -> per user php-fpm pool
/var/webapp/ferdi/files           -> actual php or htmlfiles
/var/webapp/ferdi/logs            -> logs for php and nginx so you can tail -f
/var/webapp/ferdi/php-fpm-session -> php session store
```

Ketikkan perintah-perintah ini:
```
$ mkdir /var/webapp
$ chmod g+s /var/webapp
$ mkdir /var/webapp/ferdi
$ chown ferdisn:nginx /var/webapp/ferdi
```

`$ chmod g+s` akan memberikan *sticky* GUID sehingga semua direktori dibawahnya akan mewarisi pengaturan group `webapp`.

Tujuan dibuat folder per user PHP-FPM adalah untuk penyekatan. Kalau nanti servernya dibagi dengan private key per user Linux, cukup `$ chmod 700` pada semua folder di bawah `/var/webapp`, sehingga setiap user tidak bisa saling melihat. Nginx tetap akan bisa melihat karena folder  webapp  semuanya punya hak akses baca tulis oleh user dalam group nginx (oke, boleh dicabut tulisnya, tapi untuk folder `logs` nginx harus tetap boleh menulis lho)

Apabila ada SELinux, tuliskan perintah ini:
```
$ semanage fcontext -a -t httpd_var_run_t "/var/webapp/ferdi/php-fpm-session(/.*)?"
$ semanage fcontext -a -t httpd_log_t "/var/webapp/ferdi/logs(/.*)?"
$ semanage fcontext -a -t httpd_sys_rw_content_t "/var/webapp/ferdi/files(/.*)?"
$ restorecon -Rv /var/webapp/ferdi
```
*Note: update ini untuk setting SELinux yang ada wildcardnya*

*I'd really like to explain about this thing, but for now accept as it is, ya. - Oaken*

Terkait SELinux juga, saat menyalin berkas sertifikat ke komputer tujuan, pastikan berkasnya langsung mendarat di folder `/etc/nginx`. Agar Nginx bisa diizinkan membaca berkas sertifikat, berkas itu harus berlabel `httpd_config_t`. Kalau Anda seperti saya, menaruh berkasnya di `/etc/ssl` agar bisa dipakai selain Nginx, jangan lupa relabel ke `httpd_config_t` dengan `$ semanage fcontext`  tersebut. Kemungkinan kalau ada program lain yang ingin membaca akan runyam karena label tersebut, namun, berhubung di sini cuma ada Nginx, sepertinya masih aman. [note]Butuh pengujian lanjut.[/note]

### 2. Modifikasi `/etc/hosts`
Sunting `/etc/hosts`, tambahkan domain yang dipakai untuk situs Anda secara daring di sini, agar laptop Anda pertama menghubungi Nginx di laptop lokal, bukannya mencari secara daring.
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
#::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.1	blog.ferdi.id
```
Sesudah mengatur stanza Nginx, menambahkan *database* (penting untuk konfigurasi Wordpress), dan tentunya mengatur PHP-FPM sudah dipastikan benar serta harmonis antara ketiganya, lanjut ke proses instalasi Wordpress lokal.

### 3. Instalasi Wordpress
Dengan *user* Linux yang sudah ditetapkan di *pool* PHP-FPM,silakan buat direktori untuk menampung Wordpress, tentu seperti yang ditunjuk di subdirektif `root` di bawah `server` dalam file dotNGINX Anda di `sites-available` (atau `sites-enabled`, kalau sudah di*symlink*)

Masuk ke direktori itu, dan *download* Wordpress dari http://wordpress.org dengan cara berikut:

`$ curl -O https://wordpress.org/latest.tar.gz` 

*Unpack*, lalu dari folder `wordpress`, keluarkan filenya ke `root` website.
```
$ tar xf latest.tar.gz
$ cd wordpress
$ mv * ../
$ cd ..
$ rm wordpress
```
Lalu buka browser Anda.

<img class="image" src="{{ site.asseturl }}/foto_resume-1.png" alt="" width="100%" height="100%">
<!-- ![alt text]({{ site.asseturl }}/foto_resume-1.png "Logo Title Text 1") -->

Selesaikan instalasi Wordpress.

Selesai di laptop, sebenarnya Anda melakukan hal yang sama persis di remote. Semua langkah ini Anda ulang.

Kemudian, buat private-public key untuk ssh, pastikan saat membuat tidak dilengkapi passphrase, kalau tidak proses Rsync akan terganggu pertanyaan passphrase itu.

Sangkutkan public key ke user Linux selain root yang Anda pakai di remote. Sangkutkan = tambahkan ke `~/.ssh/authorized_keys`. Pastikan Anda sudah bisa ssh manual dari user Linux selain root yang ada di laptop.

Selanjutnya, ikuti petunjuk script sinkronisasi.

Congrats yaaa.