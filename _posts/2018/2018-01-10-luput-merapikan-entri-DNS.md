---
title:  "Luput merapikan Entri DNS"
date:   2018-01-10 05:53:00 +0700
short_desc : '<p>Migrated data</p>'
tag:
 - administration
 - dns
---

*Dear Users*, jangan sampai lupa untuk mengatur entri MX dalam DNS domain Anda dengan benar.

Pada saat ini saya bergantung dengan email untuk pekerjaan. Saya baru menyadari ketika rekan koordinator dari Filipina *ngeWhatsapp* dan menanyakan apakah saya punya email lain.

Ternyata entah sejak kapan ada dua entri MX di domain ferdi.id saya. Satu menuju Capella, satu menuju Google. Tentu saja, apabila mail server Anda sedang sial dan memutuskan mengontak Google, akan muncul pesan kesalahan balasan dari Google yang menyatakan kalau "mailbox ini tidak ada di sini." Tentu saja tidak ada, mailbox itu ada di Capella.

Hapus entri MX ke Google. Selanjutnya, biar <span title="Ini kata bahasa Indonesia, bukan?" style="text-decoration:underline solid;">propagasi</span> DNS yang mengatur.