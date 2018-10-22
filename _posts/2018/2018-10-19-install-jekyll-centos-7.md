---
title:  "Pemasangan Jekyll di CentOS 7"
date:   2018-10-19 08:08:00 +0800
short_desc : '<p>Judul terlalu hiperbolis</p>'
tag:
 - jekyll
 - ruby
 - centos7
 - administration
 - learn
header_image: www/jekyll/jekyll.svg
---

Jekyll adalah *static site generator* alias mesin cetak untuk situs. Situs Anda sebenarnya HTML statis, tapi ada program yang membantu mencetaknya, alih-alih menyusun kode HTML secara manual. Jekyll ditulis dengan bahasa Ruby, sayangnya versi Ruby yang ada di repo CentOS 7 jauh di bawah persyaratan minimum Jekyll. Ini adalah catatan langkah pemasangan Ruby di CentOS 7 dengan cara di luar manajemen paket resmi CentOS 7.

Perlu diperhatikan bahwa prosedur instalasi di luar manajemen paket resmi sistem operasi Anda akan membatalkan garansi.

Tentunya itu hanya bercanda. *Seriously, though,* yang paling saya suka dari pemakaian sistem operasi Linux adalah manajemen paketnya. Saya tidak perlu memikirkan soal kompilasi perangkat lunak, atau *dependency*, dan semacamnya. Semua tinggal pakai.

Namun, kadang semua tidak semulus yang diinginkan, terutama kalau Anda ingin memakai program yang versinya lebih baru. Versi program dalam repo biasanya tertinggal jauh dibanding versi yang ada di *upstream*, apalagi kalau Anda pakai *long-term distro* seperti CentOS. Sekali waktu, saya harus berburu repo PHP7 di tempat lain karena menghindari kompilasi sendiri. Dan kali ini, terpaksa kita melakukan hal serupa.

Program yang akan dipakai untuk mengelola Ruby adalah Ruby enVironment Manager alias RVM.

[Install instruction for RVM](https://rvm.io/rvm/install)

1. Import GPG
2. Jalankan perintah instalasi pake user yang punya hak akses sudo
3. `$ rvm list known`
4. `$ rvm install ruby` -> otomatis install yang terkini
5. `$ gem install jekyll bundler` -> [QuickStart Jekyll][Jekyll]

Setelah semua perintah ini dijalankan, maka Anda sudah punya Jekyll, Ruby, RVM di distro kesayangan Anda.

---
###### Sumber:

* [Official Ruby Installation Guide](https://www.ruby-lang.org/en/documentation/installation/)
* [Ruby enVironment Manager](http://rvm.io/)

[Jekyll]: https://jekyllrb.com/docs/
