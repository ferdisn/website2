---
title:  "Permasalahan koneksi dari WildFly 14 ke PostgreSQL"
date:   2018-10-31 14:17:00 +0800
short_desc : '<p>Takes me two days</p>'
image: 
tag:
 - wildfly
 - postgresql
---

Merujuk ke tautan ini: <https://stackoverflow.com/questions/44984640/wildfly-postgres-datasource-remote-connection-url-ignored>

Pengaturan DataSource WildFly tidak akan memakai connection-url, tetapi bagian dari connection properties.

Menyusul screenshot. Sekarang yang penting koneksi dari App Server ke PostgreSQL sudah terbentuk.