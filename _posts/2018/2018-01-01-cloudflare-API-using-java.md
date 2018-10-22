---
title:  "Program berbasis Java untuk manipulasi API CloudFlare"
date:   2018-01-01 04:13:00 +0700
short_desc : '<p>Migrated data</p>'
tag:
 - administration
 - cloudflare
 - dns
---

*Frohes neues Jahr!*

Pagi ini saya baru menyelesaikan program Java untuk mengubah entri DNS di CloudFlare. CloudFlare itu sendiri punya beragam fasilitas, tapi yang saya pakai cuma fitur DNS saja. Pada zaman sebelum Masehi saya memakai FreeDNS dan juga DNS dari registrar domain, tapi sekarang-sekarang ini lebih nyaman di CloudFlare saja. Tidak ada alasan persis kenapa saya pakai CloudFlare, selain karena ini DNS pertama yang bebas dan gratis serta cukup untuk kebutuhan saya.

Apa pasal membuat program Java tersebut? Saya punya banyak nama host yang harus didaftarkan ke sertifikat, agar gembok https di peramban Chrome, Firefox, dan sebagainya, bisa berwarna hijau, bukannya menampilkan peringatan tidak aman. Karena jumlah hostname yang diverifikasi ada banyak, membuka satu-satu DNS Record ke panel kendali CloudFlare sangat melelahkan.

Dapatlah ide untuk membuat program Java untuk memanggil API CloudFlare. Silakan periksa di sini untuk melihat kode sumbernya. Saat ini program Java ini hanya untuk mengubah entri yang sudah ada sebelumnya. Tapi itu sudah lebih dari cukup, dibandingkan harus membuka satu-satu data domain untuk mengubah nilai entri.