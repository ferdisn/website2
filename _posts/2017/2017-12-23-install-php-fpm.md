---
title:  "Instalasi PHP-FPM"
date:   2017-12-23 12:13:00 +0700
short_desc : '<p>Migrated data</p>'
image: 
tag:
 - administration
 - php-fpm
---

### Persyaratan:
Fedora 26 / CentOS 7
Sambungan internet


### Langkah:
Pasang!
`# dnf install php-fpm`
Apabila belum ada dnf, silakan gunakan  yum.

### Konfigurasi Utama!
Pergi ke direktori berkas konfigurasi `/etc/php-fpm.d`.

Setiap berkas dalam direktori ini akan menjadi satu pool PHP-FPM. Kalau Anda jalankan `$ ps aux | grep php` , Anda akan dapatkan hasil berikut:
```
[ferdisn@aldebaran php-fpm.d]$ ps aux | grep php
root     11682  0.1  0.2 330124 20716 ?        Ss   18:13   0:00 php-fpm: master process (/etc/php-fpm.conf)
ferdisn  11683  0.0  0.1 342676 12004 ?        S    18:13   0:00 php-fpm: pool ferdi
ferdisn  11684  0.0  0.1 342676 12004 ?        S    18:13   0:00 php-fpm: pool ferdi
ferdisn  11685  0.0  0.1 342676 11796 ?        S    18:13   0:00 php-fpm: pool ferdi
ferdisn  11686  0.0  0.1 342676 11908 ?        S    18:13   0:00 php-fpm: pool ferdi
ferdisn  11687  0.0  0.1 342676 12008 ?        S    18:13   0:00 php-fpm: pool ferdi
ferdisn  11697  0.0  0.0 119448   968 pts/0    S+   18:13   0:00 grep --color=auto php
```
Semua proses itu adalah turunan dari setiap berkas yang ada di folder `php-fpm.d`. Demi alasan keamanan, Anda bisa buat pool per user yang memiliki aplikasi, jadi, PHP bisa disekat per user UNIX. Keuntungannya, Anda bisa modifikasi semua berkas PHP sebagai user tersebut serta tidak perlu melakukan sebagai root. Namun, penambahan pool akan  berdampak pada pemakaian memori, jadi, waspadalah!

Salin berkas contoh `www.conf` ke `ferdi.conf` lalu sunting berkas baru itu.
```
cd /etc/php-fpm.d
cp www.conf ferdi.conf
mv www.conf www.conf.bak
```
Lalu silakan sunting `ferdi.conf`.
```
; Start a new pool named 'www'.
; the variable $pool can we used in any directive and will be replaced by the
; pool name ('www' here)
[ferdi] ; REPLACE_HERE

; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
; RPM: apache user chosen to provide access to the same directories as httpd
user = ferdisn ; REPLACE_HERE
; RPM: Keep a group allowed to write in log dir.
group = ferdisn ; REPLACE_HERE

; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
;                            a specific port;
;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses
;                            (IPv6 and IPv4-mapped) on a specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
listen = /run/php-fpm/ferdi.sock ; REPLACE_HERE

; Set permissions for unix socket, if one is used. In Linux, read/write
; permissions must be set in order to allow connections from a web server.
; Default Values: user and group are set as the running user
;                 mode is set to 0660
listen.owner = nginx
listen.group = nginx
;listen.mode = 0660

; When POSIX Access Control Lists are supported you can set them using
; these options, value is a comma separated list of user/group names.
; When set, listen.owner and listen.group are ignored
listen.acl_users = apache,nginx
;listen.acl_groups =

; Additional php.ini defines, specific to this pool of workers. These settings
; overwrite the values previously defined in the php.ini. The directives are the
; same as the PHP SAPI:
;   php_value/php_flag             - you can set classic ini defines which can
;                                    be overwritten from PHP call 'ini_set'. 
;   php_admin_value/php_admin_flag - these directives won't be overwritten by
;                                     PHP call 'ini_set'
; For php_*flag, valid values are on, off, 1, 0, true, false, yes or no.

; Defining 'extension' will load the corresponding shared extension from
; extension_dir. Defining 'disable_functions' or 'disable_classes' will not
; overwrite previously defined php.ini values, but will append the new value
; instead.

; Note: path INI options can be relative and will be expanded with the prefix
; (pool, global or @prefix@)

; Default Value: nothing is defined by default except the values in php.ini and
;                specified at startup with the -d argument
;php_admin_value[sendmail_path] = /usr/sbin/sendmail -t -i -f www@my.domain.com
;php_flag[display_errors] = off
php_admin_value[error_log] = /var/webapp/ferdi/logs/php-fpm-error.log ; REPLACE_HERE
php_admin_flag[log_errors] = on
;php_admin_value[memory_limit] = 128M

; Set the following data paths to directories owned by the FPM process user.
;
; Do not change the ownership of existing system directories, if the process
; user does not have write permission, create dedicated directories for this
; purpose.
;
; See warning about choosing the location of these directories on your system
; at http://php.net/session.save-path
php_value[session.save_handler] = files
php_value[session.save_path]    = /var/webapp/ferdi/php-fpm-session; REPLACE_HERE
php_value[soap.wsdl_cache_dir]  = /var/lib/php/wsdlcache
;php_value[opcache.file_cache]  = /var/lib/php/opcache
```
Di berkas konfigurasi ini saya biasanya sertakan kata REPLACE_HERE untuk mempermudah saya ketika hendak membuat pool baru. Saya tinggal cari string ini untuk mengetahui baris mana yang harus saya ubah.

Baris berisi `php_admin_value[error_log]` dan `php_value[session.save_path]` ini harus diisikan path menuju folder yang ada di `/var/webapp/$USER/`. `error_log`  akan menampung peristiwa kesalahan yang dialami PHP-FPM saat mengeksekusi berkas PHP. `session` berguna untuk menampung session (duh!) ketika user melakukan log in dan program PHP mesti menyimpan nilai sesi ini di memori. Ini back endnya, I assume.

Baris yang berisi `listen = /run/php-fpm/ferdi.sock;` ini penting karena ini akan Anda lekatkan ke pengaturan di stanza Nginx untuk server Anda yang berbunyi `fastcgi_pass unix:/run/php-fpm/ferdi.sock;`. Tanpanya, Nginx tidak akan tahu harus menyerahkan berkas PHP ke mana.

Sudah itu saja. Proses ini tidak selalu sering terjadi, jadi tidak ada konfigurasi operasionalnya. Silakan restart PHP-FPM dengan `# systemctl restart php-fpm` untuk menarik perubahan terbaru.