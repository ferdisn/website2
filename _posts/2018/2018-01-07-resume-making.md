---
title:  "Pembuatan resume untuk lamaran kerja"
date:   2018-01-07 08:00:00 +0700
short_desc : '<p>Migrated</p>' 
tags:
- resume
- scribus
---
Saya baru menghabiskan dua hari membuat resume baru untuk lamaran kerja.

Ini sekilas fotonya:

<img class="image" src="{{ site.asseturl }}/www/resume/foto_resume-1.png" alt="sample resume screenshot" width="80%" height="80%">
*Versi 1.0 desain baru*

Ya, ternyata memikirkan desain untuk seseorang yang tidak punya cita rasa itu sulit. Namun, akhirnya berhasil berkembang menjadi versi akhir yang ada dalam tangkapan layar.

Dalam proses pembuatan resume baru ini ada beberapa hal yang ingin saya sorot. Pertama, aplikasi yang terlibat dalam pembuatan. Kedua, alur kerja yang saya pakai.

Evolusi pembuatan resume bagi saya tentu dimulai dari Microsoft Word 95. Kemudian, perkenalan dengan LinuxMint secara total di penghujung 2008, berkat Speedy, membuat saya beralih ke OpenOffice Writer yang diteruskan TDF menjadi LibreOffice. Di penghujung kuliah diploma, saya yang kembali harus membuat resume untuk kepentingan kerja dihadapkan pada berkas ODT yang *ngga banget*. Mencari-cari sebentar, saya mendarat pada Inkscape. Tidak persis untuk membuat CV, tapi karena keyword yang saya pakai waktu itu adalah "alternatives CorelDraw", Inkscape yang muncul.

<img class="image" src="{{ site.asseturl }}/www/resume/resume_inkscape.png" alt="sample resume screenshot" width="80%" height="80%">
*Hasil karya anak baru kenal Inkscape dan mencari-cari gaya desain.*

Tak lupa, sempat dibantuin seorang sahabat, inilah versi PowerPoint dari resume saya, pasca saya terlalu pusing untuk menggarap ulang resume Inkscape.

<img class="image" src="{{ site.asseturl }}/www/resume/resume_inkscape.png" alt="sample resume screenshot" width="80%" height="80%">
*Resume PowerPoint. Cadas!*

Kemudian saya berkenalan dengan Scribus. Hasilnya jadi begini.

<img class="image" src="{{ site.asseturl }}/www/resume/resume_scribus_1.png" alt="sample resume screenshot" width="80%" height="80%">
*Scribus, versi awal*

Proses berkenalannya agak konyol. Teringat dengan keyword "Desktop Publishing Program" alias DTP, saya cari keyword tersebut di Internet. Tentu, pencarian pertama akan memberikan Adobe InDesign. Pencarian "alternative InDesign" mendatangkan Scribus. Makan waktu lagi beradaptasi dari Inkscape dengan Scribus. Saya rasa memang Inkscape punya keunggulan tersendiri, namun karena karya yang saya mau buat sebenarnya adalah  program *layoutting*, Scribus yang cocok.

Ketika sedang mengaduk-aduk kanvas Scribus yang masih kosong, saya terpikir bagaimana hendak mengelola versi dari dokumen ini. Iseng saya buka berkas SLA Scribus, dan saya temukan kalau dia menyimpan data dalam bentuk teks XML. Karena berkasnya terdiri dari teks, tentu saja sangat layak masuk ke dalam git.

<img class="image" src="{{ site.asseturl }}/www/resume/gitg.png" alt="sample resume screenshot" width="80%" height="80%">
*Isi tree git saya.*

Dengan demikian, untuk kembali ke versi dokumen tertentu, saya hanya perlu `git checkout` commit yang saya mau, tentu dalam keadaan berkas tidak dibuka oleh Scribus. Masalah berikutnya berupa mengelola aset di luar Scribus, *namely* font dan gambar.

Font dalam sistem operasi saya diatur dalam berkas `/etc/fonts/fonts.conf` dan di dalam berkas konfigurasi itu ada direktif berikut:

```xml
<!-- Font directory list -->

	<dir>/usr/share/fonts</dir>
	<dir>/usr/share/X11/fonts/Type1</dir> <dir>/usr/share/X11/fonts/TTF</dir> <dir>/usr/local/share/fonts</dir>
	<dir prefix="xdg">fonts</dir>
	<!-- the following element will be removed in the future -->
	<dir>~/.fonts</dir>

<!--
```

Saya kemudian membuat folder Fonts di `$HOME` saya dan membuat symlink `$HOME/.fonts` seperti berikut:
```bash
~$ mkdir Fonts
~$ ln -s Fonts .fonts
```
lalu menjalankan perintah `$ fc-cache -fv` agar semua font dalam folder itu dideteksi oleh sistem operasi.

Ada koleksi font menarik di Google Fonts serta tentunya FontAwesome yang memberikan simbol merek yang terkini. Dapatkan berkas OTF atau TTF tersebut, letakkan di folder Fonts yang sudah dibuat, lalu segarkan tembolok font dengan perintah sebelumnya. Tutup kemudian buka kembali program Scribus (atau program apa pun yang memakai font) dan program akan mengenali font baru Anda.

Scribus hanya mengenali font berbentuk nama lengkap font dalam berkas, jadi walaupun nama font ikut masuk kendali versi dalam git, berkas font sendiri tidak ikut. boleh saja untuk sekadar pencadangan Anda selipkan berkas font dalam folder git, tapi itu tidak bijak mengingat ukurannya kebanyakan di atas 100 KB dan akan memberatkan proses git.

Alhasil, saya hanya mencadangkan font di folder lain. Pembuatan symlink tadi juga bertujuan agar folder Font selalu ikut terangkut dalam proses pencadangan, karena kadang saya lupa menyalin folder tersembunyi yang diawali titik seperti .fonts . [note]Ke depan, sepertinya font diharapkan ada di ~/.local/share/fonts  sehingga Anda bisa buat symlink ke situ.[/note]

Berkas gambar tergolong unik karena hanya berlaku untuk satu berkas itu.  Saya memasukkan semua berkas ke dalam folder Assets, dan folder ini ikut masuk ke dalam git. Berat, memang, apalagi saat push pertama kali, namun harapannya karena berkas gambar hampir sangat jarang diubah, tidak akan ada perubahan versi.

Secara keseluruhan, ini isi folder proyek resume saya.

Isi `.gitignore`.
```
[ferdisn@aldebaran Desain_Resume]$ cat .gitignore 
*.pdf
DoNotTrack/
```

<img class="image" src="{{ site.asseturl }}/www/resume/folder_assets.png" alt="sample resume screenshot" width="30%" height="30%" style="float:left;">

<img class="image" src="{{ site.asseturl }}/www/resume/folder_donottrack.png" alt="sample resume screenshot" width="30%" height="30%" style="float:left;">

<img class="image" src="{{ site.asseturl }}/www/resume/folder_resume.png" alt="sample resume screenshot" width="30%" height="30%">

Pada gambar tersebut, di folder utama dapat terlihat ada berkas PDF yang dihasilkan oleh Scribus. Saya mengecualikan berkas pdf dari git dengan entri dalam .gitignore .

Folder DoNotTrack juga dikecualikan. Dalam folder itu, Anda bisa lihat beberapa berkas yang merupakan alat bantu inspirasi saya.

Folder Assets berisi gambar yang saya pakai dalam CV saya.

Perintah git yang dieksekusi untuk mengelola berkas:

1. `git init` untuk inisialisasi
1. `touch .gitignore` lalu `vi .gitignore` lalu sunting to your heart's content.
1. `git add .` untuk menambah berkas yang belum masuk ke git.
1. `git status` untuk cek kondisi yang akan dicommit.
1. `git commit` untuk menyimpan perubahan.
1. `git remote add origin https://gitlab.com/jalur/ke/project.git` untuk mendaftarkan repository jarak jauh, jangan lupa siapkan dulu di dasbor GitLab (atau GitHub, thy choice).
1. `git push` agar commit Anda mendarat di repo jarak jauh.
1. `git push --tags` agar tag Anda juga mendarat di repo jarak jauh.

Nomor 1 sampai 5 adalah yang paling penting ketika sedang menyunting dokumen Scribus. Menyimpan setiap saat memang diperlukan. Untuk versioning, jangan lupa melakukan commit setiap kali Anda merasa bentuk dokumen Anda layak difreeze. Menurut saya, tambahan kecil pun tetap dicommit apabila tambahan itu merupakan tambahan yang akan ada secara permanen dan hanya akan dihapus apabila ada perubahan besar.



Alasan saya menyimpan proyek resume ini di GitLab cukup sederhana, GitHub tidak menyediakan repo tertutup secara gratis. Karena saya tidak mau dokumen CV saya yang sering berisi data sensitif tersebar ke mana-mana, saya menyimpannya di GitLab. Anda bisa juga menyimpan proyek git Anda di Visual Studio Team Services, juga gratis untuk repo pribadi.