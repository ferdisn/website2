---
title:  "Spring Framework Study: The Beginning"
date:   2018-10-02 08:08:00 +0800
short_desc : '<p>Judul terlalu hiperbolis</p>'
tag:
 - spring
 - learn
---

Proyek yang digarap dalam artikel ini (bukan dalam seri ini) dibangun di atas Java 10 dan IntelliJ IDEA.

Buat berkas proyek di [sini](https://start.spring.io/).

<sup>Alternatively, go [here](https://repo.spring.io/release/org/springframework/spring/) and create your project manually.</sup>

Mekarkan berkas zip, letakkan di suatu tempat, dan buka dengan IntelliJ. Coba jalankan.

TODO: screenshot here.

Apabila ada error reflective access, tambahkan argumen `--add-opens java.base/java.lang=ALL-UNNAMED` ke *VM Options* yang ada di **Run > Edit Configurations...** atau jalan pintasnya di dropdown runner yang ada di kanan atas  IntelliJ. Referensi: [IntelliJ config](https://www.jetbrains.com/help/idea/setting-configuration-options.html), [Stack Overflow answer](https://stackoverflow.com/questions/46671472/illegal-reflective-access-by-org-springframework-cglib-core-reflectutils1).

I'm following this tutorial which are made for older version of spring [here](https://www.tutorialspoint.com/spring/spring_hello_world_example.htm). It asks for xml config instead of annotation. The file i got from spring boot, requires me to put this file under resources.

https://www.youtube.com/watch?v=h7IdaLybg9A&list=PLE37064DE302862F8&index=5

https://www.tutorialspoint.com/spring/aop_with_spring.htm