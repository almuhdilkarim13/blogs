---
draft     : false
title     : "installasi mariadb menggunakan docker"
date      : 2021-12-26
lastmod   : ""
thumbnail : ""
apps      : "docker"
programing: ""
related1  : "docker"
related2  : "mariadb"
related3  : "arch"
related4  : "DevOps"
categories: [ DevOps ]
tags      : [ docker, mariadb ]
math      : false
article :
    status    : true
    thumbnail : ""
    teaser    : ""
slide :
    status   : false
    event    : ""
    organize : ""
audio :
    status   : false
    url      : ""
    sub_id   : ""
    sub_en   : ""
    desc     : ""
video :
    status   : false
    videoid  : ""
    desc     : ""

seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Menginstall lynis dengan mudah dengan berbaagai metode dan untuk berbagai distro linux "
---

## Mengunduh Images

Untuk installasi mariadb menggunakan docker kita bisa menggunakan perintah

```
docker pull mariadb
```

Untuk melihat apakah images mariadb telah terunduh atau belum, kita bisa menggetikan perintah berikut di terminal

```
docker images | grep mariadb
```

Jika images sudah terunduh maka output dari command tersebut seharusnya sebagai berikut

```
[null@localhost~]$ docker images | grep mariadb
mariadb      latest    e2278f24ac88   6 weeks ago   410MB
```


## Membuat docker Volume

Volume di butuhkan untuk membuat data tersimpan secara permanen. Nah dalam tutorial ini kita akan membuat volume untuk pengembangan c_module_user,sehingga untuk mempermudah kita akan menyamkan namanya

```
docker volume create c_module_user
```

Selanjutnya kita akan mengecek apakah volume tersebut telah berhasil dibuat atau belum dengan perintah :

```
docker volume list | grep 'c_module_user'
```

Output dari command tersebut seharusnya adalah
```
[null@samurai modul_user]$ docker volume list | grep c_module_user
local     c_module_user
```



## Membuat Container

Setelah images terunduh, langkah selanjutnya adalah membuat container dari images yang telah di unduh.
Untuk membuat container dari mariadb, kita bisa mengetikan perintah berikut

```
docker run --name c_user_module -e MYSQL_ROOT_PASSWORD=pass -p 3308:3308 -d mariadb:latest
```



