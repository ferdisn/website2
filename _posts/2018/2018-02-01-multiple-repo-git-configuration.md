---
title:  "Pengaturan banyak repo git untuk pencadangan dokumen konfigurasi"
date:   2018-02-01 08:08:00 +0800
short_desc : '<p>Migrated data</p>'
tag:
 - git
 - learn
---


<a href="https://stackoverflow.com/questions/849308/pull-push-from-multiple-remote-locations" target="_blank">source</a>

Untuk keperluan backup config, lakukan hal berikut.

setel repository

```
$ git init
$ git add .
$ git commit
```

Set upstream dengan contoh seperti berikut:

```
$ git remote add origin https://github.com/ferdisn/rpmbuild.git
$ git remote set-url origin --add https://gitlab.com/ferdisn/rpmbuild.git
$ git remote -v
```

Selanjutnya, `$ git push origin master`  untuk pertama kali, `$ git push` untuk kali berikutnya.

Contoh berkas konfigurasi  yang sudah jadi:
```
[root@capella rpmbuild]# cat .git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://github.com/ferdisn/rpmbuild.git
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = https://gitlab.com/ferdisn/rpmbuild.git
[root@capella rpmbuild]# 
```

Harus diingat juga karena ini untuk metode pencadangan data, tidak akan ada kolaborasi di dalamnya. Jangan lupa untuk siapkan berkas `.gitignore` sehati-hati mungkin agar tidak ada dokumen terkait credential yang terangkut ke repo publik di tempat tujuan Anda. *Genieß es!*   