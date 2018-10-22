---
title:  "Blokir akses dengan FirewallD"
date:   2018-01-10 05:43:00 +0700
short_desc : '<p>Migrated data</p>'
tag:
 - administration
 - firewall
---

Ketika saya menyadari ada beberapa serangan yang masuk ke server Postfix, saya sadar kalau saya harus menghalaunya. Sebelumnya saya pakai metode blokir di tingkat SMTP. Namun, kali ini saya akan pakai FirewallD. FirewallD adalah... cek <a href="http://www.firewalld.org/" target="_blank">di sini</a>. Cukup katakan saja ini tembok api yang ada di dalam distribusi GNU/Linux Fedora dan turunannya.

Memblokir dengan cara ini:
```
# firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='115.239.228.12' reject"
```
Ganti `--add`  dengan `--remove` untuk menghapus *rule* ini.

Jangan lupa jalankan `# firewall-cmd --reload` untuk mengaktifkan semua perubahan yang dibuat.

Ini isi skema tembok api:
```
[root@capella postfix]# firewall-cmd --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces: 
  sources: 
  services: dhcpv6-client imaps smtp ssh
  ports: 587/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  sourceports: 
  icmp-blocks: 
  rich rules: 
	rule family="ipv4" source address="78.157.216.211" reject
	rule family="ipv4" source address="121.201.38.118" reject
	rule family="ipv4" source address="27.50.138.69" reject
	rule family="ipv4" source address="115.59.74.249" reject
	rule family="ipv4" source address="213.7.202.194" reject
	rule family="ipv4" source address="185.227.108.59" reject
	rule family="ipv4" source address="185.227.108.10" reject
	rule family="ipv4" source address="115.160.177.22" reject
```
Bagian *services* dan *ports* untuk pengaturan *port* mana yang dibuka. Apabila *port* sudah ada definisi *service*nya dalam FirewallD, bisa pakai *services*. Kalau belum, bisa pakai *port*. Yang terbuka di server ini adalah imaps (993), smtp (25), ssh (22), dan *port* 587 untuk *submission* surat elektronik.

Di bagian *rich rules* ada barisan kode yang diinput dengan parameter `--add-rich-rule` tadi.

Memang akan terjadi kepusingan jika daftar ini bertambah, tapi semoga ketika kepusingan itu datang, saya bisa pakai alat pengatur jaringan yang lebih mumpuni. Amin.

Link ke list ip block: [here](http://ipdeny.com/ipblocks/)

Link untuk cek ownership detail soal IP: [here](https://mxtoolbox.com/SuperTool.aspx?action=arin%3a115.48.0.0%2f12&run=toolpage#)

Link utk location IP: [here](https://www.iplocation.net/)