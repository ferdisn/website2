---
title:  "Konfigurasi Repo Extra Packages for Enterprise Linux (EPEL)"
date:   2017-12-30 13:17:00 +0700
short_desc : '<p>Migrated data</p>'
tag:
 - administration
 - centos7
 - epel
---

#### Apa itu Extra Packages for Enterprise Linux (EPEL)?
<a href="https://fedoraproject.org/wiki/EPEL" target="_blank">Di sini</a> ada situs utama untuk Extra Packages for Enterprise Linux, selanjutnya disebut EPEL. Jujur, saya tidak paham kemunculan EPEL ini sendiri. Yang saya tahu, Nginx tidak ada di repo CentOS, tidak seperti Fedora. Oleh karena itu, apabila sistem operasi Anda adalah RHEL dan turunannya, seperti CentOS, ikuti langkah berikut untuk konfigurasi.

#### Instalasi Konfigurasi
`# yum install epel-release`

Jawab "YES" atau "JA" (tergantung lokalisasi sistem operasi) ke semua pertanyaan. Kebanyakan isinya soal kunci GPG.

Selesai.



Ya, seperti itu saja.

Selanjutnya, Anda bisa pakai Yum untuk instalasi Nginx di CentOS 7, seperti halnya DNF di Fedora.

#### Instalasi Konfigurasi Secara Manual
Jangan menyusahkan diri Anda. Ikuti saja yang sudah diterangkan sebelumnya.

Instalasi manual sebenarnya melibatkan pembuatan berkas konfigurasi repo di folder `/etc/yum.repos.d/`. *See for yourself*:

![isi-RPM](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Isi RPM epel-release")

Berkas di screenshot tersebut hanya bisa dibuka sebagai user root. Percayalah, ambil cara otomatis.

Kalau masih mau manual, *here it is*.

```
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
failovermethod=priority
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7

[epel-debuginfo]
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch/debug
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1

[epel-source]
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
#baseurl=http://download.fedoraproject.org/pub/epel/7/SRPMS
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-source-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1
```
Lupa-lupa ingat, kalau tidak salah GPG harus pasang manual juga kalau pengaturan repo dilakukan secara manual.