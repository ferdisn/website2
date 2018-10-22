---
title:  "Script sinkronisasi Wordpress lokal ke remote"
date:   2017-12-30 12:13:00 +0700
short_desc : '<p>Migrated data</p>'
image: 
tag:
 - wordpress
 - bash
---
```
#!/bin/bash

ssh_command="ssh -i ~/.ssh/to_bellatrix_ferdi"
cd ~/WebDir/files/blog.ferdi.id
mysqldump -u DBUserLokal -pDBPasswordLokal DBNameLokal > namafile.sql
rsync -azPrv --delete -e "$ssh_command" /var/webapp/ferdi/files/blog.ferdi.id/ ferdi_bellatrix:/var/webapp/ferdi/files/blog.ferdi.id/
ssh ferdi_bellatrix "sed -i 's/DBUserLokal/DBUserRemote/g' /var/webapp/ferdi/files/blog.ferdi.id/wp-config.php"
ssh ferdi_bellatrix "mysql -u DBUserRemote -pDBPasswordRemote DBNameRemote < /var/webapp/ferdi/files/blog.ferdi.id/namafile.sql "`
```
Penjelasan perbaris.

Baris 3, buat variabel ssh_command  untuk menampung perintah ssh -i ~/.ssh/to_bellatrix_ferdi  yang akan dipakai rsync . Parameter -i  di program Ssh berguna untuk memberikan private key tertentu untuk sambungan ssh. Private key standar yang dipanggil oleh aplikasi Ssh adalah id_rsa , namun, bagaimana kalau Anda punya puluhan private key?

Baris 4, pergi ke direktori utama Wordpress. Jangan bingung dengan peletakannya di WebDir. Saya membuat symlink dari /var/webapp/ferdi  ke ~/WebDir . Demi kepraktisan sehari-hari saja.

Baris 5, dump database blog ke berkas. Karena baris empat, maka berkas ini akan ada di lokasi direktori baris 4. File ini akan terbawa ke remote berkat Rsync di baris 6.

Baris 6, sinkronisasi ke remote. Saya lupa catatannya, yang jelas switch yang saya pakai di Rsync ini yang terbaik untuk sinkronisasi dari lokal ke remote (atau sebaliknya). Baris ini memakai informasi di baris 1. Kata ferdi_bellatrix  di argumen terakhir perintah rsync sebenarnya adalah alias yang ada di ~/.ssh/config . Di sana sudah ada informasi soal private key yang dibutuhkan, tapi Rsync tidak baca, jadi, harus pakai variable ssh_command.

Baris 7, terjadi karena waktu membuat user untuk database remote, saya tidak bisa pakai user yang sama dengan yang di lokal karena terlalu panjang. Sepertinya karena perbedaan versi antara MariaDB laptop (Fedora 26) dengan remote (CentOS 7). Jadi, karena ada perbedaan nama user, kalau konfigurasi Wordpress lokal diangkut ke remote melalui Rsync, Wordpress remote tidak akan bisa tersambung ke databasenya karena nama user yang berbeda. Baris ini untuk memodifikasi konfigurasi Wordpress remote agar tersambung. Baris ini bisa dihapus kalau konfigurasi di lokal dan remote sama persis.

Baris 8, kembalikan isi dumpfile ke database. Namanya harus sama dengan di baris 5.

Das ist alles.