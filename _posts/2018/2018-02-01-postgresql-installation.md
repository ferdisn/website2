---
title:  "Instalasi PostgreSQL di CentOS 7"
date:   2018-02-01 08:08:00 +0800
short_desc : '<p>Migrated data</p>'
tag:
 - centos7
 - postgresql
---


https://stackoverflow.com/questions/30617357/unable-to-connect-to-postgres-via-php-but-can-connect-from-command-line-and-pgad

pgpass

https://www.postgresql.org/docs/current/static/backup-dump.html

https://www.postgresql.org/download/linux/redhat/

Instalasi langsung dari repo Yum PostgreSql biar dapat versi terkini.

Setelah pilih versi OS, jalankan perintah `# yum install https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-1.noarch.rpm`
Install package server `# yum install postgresql10-server`
Melengkapi
```
# /usr/pgsql-10/bin/postgresql-10-setup initdb
# systemctl enable postgresql-10
# systemctl start postgresql-10
```
Selesai
User non default yang berpassword:

https://stackoverflow.com/questions/10757431/postgres-upgrade-a-user-to-be-a-superuser

https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e

* Berubah jadi user postgres (*macam pahlawan saja*) dengan `# su - postgres`
* masuk dengan `$ psql`
* ketik `create user with superuser;`
* kasih password `alter user <username> with encrypted password '<password>';`

Allow password connection:

* Edit `/var/lib/pgsql/10/data/pg_hba.conf`
* ubah METHOD dari ident ke password untuk kedua row host itu.

```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            password
# IPv6 local connections:
host    all             all             ::1/128                 password
```
* restart postgre dengan `# systemctl restart postgresql-10`
Berikutnya atur tunneling dari local ke sana dengan `ssh -L 5433:localhost:5432 root_bellatrix` *and you can do the rest with PgAdmin*.