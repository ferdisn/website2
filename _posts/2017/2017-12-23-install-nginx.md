---
title:  "Pemasangan Nginx"
date:   2017-12-23 12:13:00 +0700
short_desc : '<p>I can show you the world wide web</p>'
image: 
tag:
 - administration
 - nginx
 - centos7
---

### Persyaratan:
* Fedora 26 / CentOS 7 <https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7>
* Sambungan internet


### Langkah:
**Pasang!!!**

`# dnf install nginx`

Apabila belum ada dnf, silakan gunakan  yum. Apabila pakai Yum, sepertinya Anda di *distro* turunan RHEL. Ikuti panduan di [sini](/2017/12/30/epel-configuration.html) untuk konfigurasi repo EPEL.

### Konfigurasi Utama!
Sunting berkas konfigurasi `/etc/nginx/nginx.conf`.
```
.
.
.
    include /etc/nginx/conf.d/*.conf;

	client_max_body_size 999M;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

	include /etc/nginx/sites-enabled/*.nginx;

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
.
.
.
```
`client_max_body_size` berfungsi untuk memastikan Nginx tidak *timeout* ketika peramban mengirim berkas besar. Pengalaman waktu pakai Zimbra dulu, kalau lagi mau kirim lampiran, cenderung *timeout*. Namun, kalau *ga perlu-perlu amat*, silakan kecilkan angkanya jadi 5M.

`include /etc/nginx/sites-enabled/*.nginx;` ini fungsinya agar Nginx membaca konfigurasi di folder `/etc/nginx/sites-enabled` yang berekstensi dotNGINX. Nanti seluruh stanza *virtual host* akan diletakkan di sini. Jadi, tidak perlu mengubah berkas utama nginx.conf secara langsung.

Sebagai tambahan, stanza *server* yang ada di atas akan membuat pengunjung yang hanya mengunjungi IP *server* Anda atau mengunjungi domain yang tidak valid menurut konfigurasi Nginx, melihat halaman dari stanza ini. Silakan komentari dengan tanda pagar kalau tidak mau hal itu terjadi. Pilihan lainnya adalah membuat redirect dari HTTP ke HTTPS untuk semua kunjungan berbentuk domain. <sup>In time, I'll make it.</sup>

Pengoperasian! (Penambahan dan penghapusan stanza)
Setiap kali hendak membuat stanza baru, bebas mau menyusunnya seperti apa. Apakah dimasukkan ke satu file dotNGINX tunggal, atau per domain, atau per aplikasi. Saya belum *maintain* banyak, jadi masih cenderung per domain atau per kepemilikan.

Contoh stanza dari berkas ferdi.nginx:
```
# stanza ini akan membuat akses menuju server_name di port 80
# dialihkan ke server_name yang sama dengan protokol https
# bahasa kerennya HTTP to HTTPS redirect

server {
    listen 80;
    server_name blog.ferdi.id;
    return 301 https://$host$request_uri;
}

# stanza ini adalah konfigurasi server yang asli

server {
    listen 443 ssl; 
    server_name blog.ferdi.id;

## SSL certificate dari Let's Encrypt

    ssl_certificate /etc/ssl/fullchain.pem;
    ssl_certificate_key /etc/ssl/privkey.pem;

## Lokasi berkas log untuk Nginx

    access_log /var/webapp/ferdi/logs/blog.ferdi.id_access.log;
    error_log /var/webapp/ferdi/logs/blog.ferdi.id_error.log;

## lokasi berkas situs yang akan disajikan
    root /var/webapp/ferdi/files/blog.ferdi.id;

## berkas yang akan dicari pertama kali kalau di peramban tidak disebutkan secara spesifik
    index index.php index.html index.htm;

## forwarder untuk PHP
	location / {
		try_files $uri $uri/ /index.php?$args;
	}

    charset utf-8;

## yang melempar pemrosesan ke PHP-FPM
	location ~ ^(.+\.php)(.*)$ {
		try_files $fastcgi_script_name =404;
		#include        /etc/nginx/fastcgi_params;
		include fastcgi_params;
		fastcgi_split_path_info  ^(.+\.php)(.*)$;
		fastcgi_pass unix:/run/php-fpm/ferdi.sock;
		fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
		fastcgi_param  PATH_INFO        $fastcgi_path_info;
	}
}
```
Setelah berkas disimpan di `sites-available`, Anda tinggal membuat symlink di `sites-enabled`.
```
cd /etc/nginx/sites-enabled
ln -s ../sites-available/ferdi.nginx
```
Setelah symlink dibuat, restart Nginx dengan `systemctl restart nginx` (atau reload  juga bisa, tapi saya  hampir tidak pernah pakai) agar Nginx melihat perubahan yang dibuat. Tujuan hal ini agar kalau sewaktu-waktu Anda perlu takedown situs dengan menghapus berkas konfigurasi dotNGINX, Anda hanya perlu menghapus symlink tersebut. Anda bisa kembalikan lagi dengan membuat ulang symlink dengan perintah seperti sebelumnya.

Terkait path yang dicantumkan di stanza tersebut, ada beberapa asumsi yang saya buat, yaitu:

* Semua berkas situs ditampung di folder `/var/webapp/$USER/files/nama_domain/`
* Berkas log ditaruh di `/var/webapp/$USER/logs`
Buatlah semua folder ini sebelum menjalankan Nginx, kalau tidak Nginx akan gagal berjalan. Pastikan juga Nginx punya hak akses baca untuk folder berkas situs serta hak akses baca tulis untuk folder logs.

Variabel $USER itu bebas, tidak harus sama dengan username UNIX. Yang penting nanti untuk PHP-FPM, pengaturan hak aksesnya benar.
