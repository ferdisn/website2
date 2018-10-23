---
title:  "Pemasangan MariaDB"
date:   2017-12-23 12:13:00 +0700
short_desc : '<p>Things get lost here</p>'
image: 
tag:
 - administration
 - mariadb
 - centos7
---

### Persyaratan:
* Fedora 26 / CentOS 7
* Sambungan internet


### Langkah
**Pasang!!!**

`# dnf install mariadb-server`

Apabila belum ada dnf, silakan gunakan yum.

### Konfigurasi Utama
Tidak ada berkas untuk disunting, kecuali Anda mau mengatur replikasi.

Nyalakan dulu *daemon* mariadb dengan `# systemctl start mariadb`.

Jalankan perintah `mysql_secure_installation` sebagai root.
```
[ferdisn@aldebaran php-fpm.d]$ sudo mysql_secure_installation 
[sudo] Passwort für ferdisn: 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```
*Script* ini saya jalankan untuk kedua kalinya (kalau tidak, artikel ini tidak akan bisa naik, :D. Nanti akan saya sempatkan coba pasang baru untuk contoh di halaman ini.) *Script* ini hanya untuk *hardening* standar saja, namun sangat bermanfaat, paling tidak agar root mariadb jadi ber*password*.

Pengoperasian! (Menambahkan *database* dan *user*)

```mysql
$ mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5340 to server version: 3.23.54
 
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
 
mysql> CREATE DATABASE DBName;
Query OK, 1 row affected (0.00 sec)
 
mysql> GRANT ALL PRIVILEGES ON DBName.* TO "DBUser"@"%" IDENTIFIED BY "DBPassword";
Query OK, 0 rows affected (0.00 sec)
  
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)

mysql> EXIT
Bye
$
```

DBName, DBUser, dan DBPassword itu yang harus Anda ganti. Kalau biasanya untuk environment laptop saya, saya buat satu string yang sama persis semua. Sangat tidak aman sih, hahahaha.

Tidak perlu *restart service* setelah menambahkan *database* atau *user*.