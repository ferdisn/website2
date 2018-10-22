---
title:  "Upgrade ke PHP 7.x di CentOS 7"
date:   2017-12-30 16:14:00 +0700
short_desc : '<p>Migrated data</p>'
tag:
 - administration
 - centos7
 - php7
---

Timbul kebutuhan memasang Laravel, sehingga versi PHP di CentOS 7 harus dinaikkan ke versi 7.x.

Sebelumnya versi PHP pernah dinaikkan ke 5.5, karena versi tertinggi PHP CentOS 7 ada di 5.4, dan Baikal sebagai aplikasi CardDAV/CalDAV saya tidak bisa berjalan di versi PHP itu. Karena peristiwa itu, konfigurasi repo Remi sudah pernah terpasang. Saya hanya perlu mengaktifkan repo versi 7.x.

Cek informasi nama repo dengan: `# yum repolist all` atau boleh juga `# yum repolist disabled` untuk memunculkan daftar repo.
```
$ yum repolist all
Geladene Plugins: fastestmirror
Determining fastest mirrors
 * base: mirror.0x.sg
 * epel: mirror.nes.co.id
 * extras: mirror.0x.sg
 * remi-php55: mirrors.thzhost.com
 * remi-php71: mirrors.thzhost.com
 * remi-safe: mirrors.thzhost.com
 * updates: mirror.0x.sg
Repo-ID                          Repo-Name:                                                               Status
C7.0.1406-base/x86_64            CentOS-7.0.1406 - Base                                                   deaktiviert
C7.0.1406-centosplus/x86_64      CentOS-7.0.1406 - CentOSPlus                                             deaktiviert
C7.0.1406-extras/x86_64          CentOS-7.0.1406 - Extras                                                 deaktiviert
C7.0.1406-fasttrack/x86_64       CentOS-7.0.1406 - CentOSPlus                                             deaktiviert
C7.0.1406-updates/x86_64         CentOS-7.0.1406 - Updates                                                deaktiviert
C7.1.1503-base/x86_64            CentOS-7.1.1503 - Base                                                   deaktiviert
C7.1.1503-centosplus/x86_64      CentOS-7.1.1503 - CentOSPlus                                             deaktiviert
C7.1.1503-extras/x86_64          CentOS-7.1.1503 - Extras                                                 deaktiviert
C7.1.1503-fasttrack/x86_64       CentOS-7.1.1503 - CentOSPlus                                             deaktiviert
C7.1.1503-updates/x86_64         CentOS-7.1.1503 - Updates                                                deaktiviert
C7.2.1511-base/x86_64            CentOS-7.2.1511 - Base                                                   deaktiviert
C7.2.1511-centosplus/x86_64      CentOS-7.2.1511 - CentOSPlus                                             deaktiviert
C7.2.1511-extras/x86_64          CentOS-7.2.1511 - Extras                                                 deaktiviert
C7.2.1511-fasttrack/x86_64       CentOS-7.2.1511 - CentOSPlus                                             deaktiviert
C7.2.1511-updates/x86_64         CentOS-7.2.1511 - Updates                                                deaktiviert
!base/7/x86_64                   CentOS-7 - Base                                                          aktiviert:  9.591
base-debuginfo/x86_64            CentOS-7 - Debuginfo                                                     deaktiviert
base-source/7                    CentOS-7 - Base Sources                                                  deaktiviert
c7-media                         CentOS-7 - Media                                                         deaktiviert
centosplus/7/x86_64              CentOS-7 - Plus                                                          deaktiviert
centosplus-source/7              CentOS-7 - Plus Sources                                                  deaktiviert
cr/7/x86_64                      CentOS-7 - cr                                                            deaktiviert
!epel/x86_64                     Extra Packages for Enterprise Linux 7 - x86_64                           aktiviert: 12.156
epel-debuginfo/x86_64            Extra Packages for Enterprise Linux 7 - x86_64 - Debug                   deaktiviert
epel-source/x86_64               Extra Packages for Enterprise Linux 7 - x86_64 - Source                  deaktiviert
epel-testing/x86_64              Extra Packages for Enterprise Linux 7 - Testing - x86_64                 deaktiviert
epel-testing-debuginfo/x86_64    Extra Packages for Enterprise Linux 7 - Testing - x86_64 - Debug         deaktiviert
epel-testing-source/x86_64       Extra Packages for Enterprise Linux 7 - Testing - x86_64 - Source        deaktiviert
!extras/7/x86_64                 CentOS-7 - Extras                                                        aktiviert:    327
extras-source/7                  CentOS-7 - Extras Sources                                                deaktiviert
fasttrack/7/x86_64               CentOS-7 - fasttrack                                                     deaktiviert
remi                             Remi's RPM repository for Enterprise Linux 7 - x86_64                    deaktiviert
remi-debuginfo/x86_64            Remi's RPM repository for Enterprise Linux 7 - x86_64 - debuginfo        deaktiviert
remi-php54                       Remi's PHP 5.4 RPM repository for Enterprise Linux 7 - x86_64            deaktiviert
!remi-php55                      Remi's PHP 5.5 RPM repository for Enterprise Linux 7 - x86_64            aktiviert:    415
remi-php55-debuginfo/x86_64      Remi's PHP 5.5 RPM repository for Enterprise Linux 7 - x86_64 - debuginf deaktiviert
remi-php56                       Remi's PHP 5.6 RPM repository for Enterprise Linux 7 - x86_64            deaktiviert
remi-php56-debuginfo/x86_64      Remi's PHP 5.6 RPM repository for Enterprise Linux 7 - x86_64 - debuginf deaktiviert
remi-php70                       Remi's PHP 7.0 RPM repository for Enterprise Linux 7 - x86_64            deaktiviert
remi-php70-debuginfo/x86_64      Remi's PHP 7.0 RPM repository for Enterprise Linux 7 - x86_64 - debuginf deaktiviert
remi-php70-test                  Remi's PHP 7.0 test RPM repository for Enterprise Linux 7 - x86_64       deaktiviert
remi-php70-test-debuginfo/x86_64 Remi's PHP 7.0 test RPM repository for Enterprise Linux 7 - x86_64 - deb deaktiviert
!remi-php71                      Remi's PHP 7.1 RPM repository for Enterprise Linux 7 - x86_64            aktiviert:    365
remi-php71-debuginfo/x86_64      Remi's PHP 7.1 RPM repository for Enterprise Linux 7 - x86_64 - debuginf deaktiviert
remi-php71-test                  Remi's PHP 7.1 test RPM repository for Enterprise Linux 7 - x86_64       deaktiviert
remi-php71-test-debuginfo/x86_64 Remi's PHP 7.1 test RPM repository for Enterprise Linux 7 - x86_64 - deb deaktiviert
!remi-safe                       Safe Remi's RPM repository for Enterprise Linux 7 - x86_64               aktiviert:  2.578
remi-safe-debuginfo/x86_64       Remi's RPM repository for Enterprise Linux 7 - x86_64 - debuginfo        deaktiviert
remi-test                        Remi's test RPM repository for Enterprise Linux 7 - x86_64               deaktiviert
remi-test-debuginfo/x86_64       Remi's test RPM repository for Enterprise Linux 7 - x86_64 - debuginfo   deaktiviert
!updates/7/x86_64                CentOS-7 - Updates                                                       aktiviert:  1.573
updates-source/7                 CentOS-7 - Updates Sources                                               deaktiviert
repolist: 27.005
```
Yang diawali tanda seru berarti reponya aktif.

Switch `--enablerepo` hanya mengaktifkan repo selama durasi perintah yang disertakan. Karena saya mau permanen di PHP 7.x, saya tidak memerlukan ini. Saya harus mengaktifkan dengan mengedit berkas konfigurasi di `/etc/yum.repos.d/`, namun, kalau Anda malas, Anda bisa pake peralatan yang tersedia.
```
# yum install yum-utils
# yum-config-manager --enable remi-php71
```
Kenapa tidak pasang berdampingan? Hahaha. Instalasi CentOSnya mulai *butchered*.

Perbarui PHP-FPM.
```
# yum update php-fpm
# systemctl restart php-fpm
```

Referensi:
* https://blog.remirepo.net/pages/Config-en
* https://superuser.com/questions/488890/list-of-installed-repositories-yum
* https://unix.stackexchange.com/questions/153110/does-yums-enablerepo-option-only-enable-a-repo-for-the-current-command