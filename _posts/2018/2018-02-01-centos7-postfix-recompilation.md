---
title:  "Kompilasi ulang Postfix di CentOS 7 untuk mendapatkan dukungan PostgreSQL"
date:   2018-02-01 08:08:00 +0800
short_desc : '<p>Migrated data</p>'
tag:
 - postfix
 - centos7
 - postgresql
---

Postfix di CentOS 7 tidak mendukung PostgreSQL. Periksalah dengan menjalankan perintah `posctconf -m`  dan apabila dalam hasilnya tidak ada `pgsql`, maka Anda bisa melakukan kompilasi ulang. Tutorial berikut untuk Centos 7.4.

Kalau berniat menjalankan kompilasi, persiapkan lingkungan dengan hal-hal berikut:
```
# yum groupinstall "Development tools"
# yum install  openssl-devel  mysql-devel pcre-devel cyrus-sasl-devel openldap-devel db4-devel zlib-devel
```
Lalu *download* SPRM untuk Postfix di [sini](http://vault.centos.org/7.4.1708/os/Source/SPackages/), mekarkan menggunakan `$ rpm -i`  dan Anda akan dapat folder `rpmbuild`  di folder $HOME Anda.

Lalu sunting berkas `~/rpmbuild/SPECS/postfix.spec`  dan ubah 3 hal berikut:
```
< %bcond_with pgsql
> %bcond_without pgsql

~%if %{with pgsql}
< CCARGS="${CCARGS} -DHAS_PGSQL -I%{_includedir}/pgsql"
> CCARGS="${CCARGS} -DHAS_PGSQL -I/usr/pgsql-10/include"
< AUXLIBS="${AUXLIBS} -lpq"
> AUXLIBS="${AUXLIBS} -L/usr/pgsql-10/lib -lpq"
~%endif
```
Direktif pertama, saya ga begitu tahu kenapa, tapi kalau tidak diganti, blok if yang berikutnya ga dieksekusi, sehingga dukungan PostgreSQL tidak masuk ke binary hasil kompilasi.
Blok if setelahnya, hanya mengarahkan *header files* PostgreSQL dan *library* tambahan yang akan di*link* ke binary PostgreSQL. CMIIW.

Lalu di direktori yang sama, jalankan perintah `$ rpmbuild -ba postfix.spec`

Kalau berjalan normal, akan ada binary RPM di `~/rpmbuild/x86_64/postfix-2.10.1-6.el7.centos.x86_64.rpm`  yang siap dipasang.

* Matikan postfix `# systemctl stop postfix`
* Backup semua file konfigurasi penting (normalnya akan dibackup oleh rpm, tapi buat jaga-jaga aja)
* Remove paket postfix dari repo CentOS `# yum remove postfix`
* Jalankan `# rpm -ivh postfix-2.10.1-6.el7.centos.x86_64.rpm` di direktori yang ada berkas RPM tadi untuk memasang paket Postfix ini
* Silakan `postconf -m` lagi untuk memeriksa kemampuan pgsql.
* Kembalikan lagi semua setting Postfix yang dibackup RPM (atau dari backup Anda)
* `# systemctl start postfix`, dan Postfix Anda sudah siap menerima akses ke PostgreSQL

Tadaaaa.