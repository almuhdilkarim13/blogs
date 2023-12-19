---
draft     : false
title     : "Membuat Aplikasi Pengguna Menggunakan C Dan Mariadb"
date      : 2021-12-26
lastmod   : ""
thumbnail : ""
apps      : "docker"
programing: ""
related1  : "docker"
related2  : "mariadb"
related3  : "arch"
related4  : "DevOps"
categories: [ Programing ]
tags      : [ C ]
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

## Requirement
Untuk berhubungan dengan mariadb, aplikasi c membutuhkan connector, dalam tutorial ini kita menggunakan connector
```
mariadb-libs 10.6.5-2
```

untuk installasinya pada arch linux bisa menggunakan perintah di bawah ini;
```
sudo pacman -S mariadb-libs
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


## cara melihat ip container yang menjalankan mariadb


```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [Nama container]
```


ganti nama container dengan container yang menjalankan mariadb, jika nama containernya `c_module_user`, maka perintah nya adalah 



```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' c_module_user
```


## Membuat configurasi docker-compose


Dalam kasus ini kita menggunakan image mariadb untuk sebagai database yang di jalankan dalam docker. Oleh sebab itu, untuk memudahkan kita, kita bisa menggunakan docker compose, berikut ini adalah file configurasi untuk docker compose

```
version: '2.2.2'

services:
  db:
    image : mariadb:latest
    container_name : c_module_user
    environment:
      MYSQL_ROOT_PASSWORD: test
      MYSQL_DATABASE: c_user_module
      MYSQL_USER: user_mod
      MYSQL_PASSWORD: test
      MYSQL_TCP_PORT: 3306
    ports:
      - "3306:3306"
    volumes:
      - c_module_user:/var/lib/mysql
volumes:
  c_module_user:
```

Untuk menjalankan configurasi tersebut, pertama sekali kita perlu membuat file configurasi dengan format .yml. dalam kasus ini kita akan membuat file dengan nama `c_module_user.yml`.


Setelah itu kita copy dan paste kofigurasi di atas kedalam file yang baru di buat dan save filenya.

setelah itu kita perlu membuild configurasi tersebut dengan command

```
docker-compose create
```


setelah proses build selesai, kita tinggal menjalankan nya dengan perintah

```
docker-compose start
```

## Membuat file program test koneksi database 

setelah aplikasi database selesai dibuat, selanjutnya kita akan membuat aplikasi sederhana untuk menguji koneksi ke database menggunakan bahasa pemograman c, pertama sekali kita buat file kosong dengan nama conn.c

selanjutnya copy dan paste script dibawah ini kedalam file yang baru saja kita buat.

```
#include <stdlib.h>
#include <stdio.h>
#include "mysql/mysql.h"

int main(int argc, char **argv[])
{
  MYSQL *con = mysql_init(NULL);

  if (con == NULL)
  {
      fprintf(stderr, "%s\n", mysql_error(con));
      exit(1);
  }


  if(mysql_real_connect(con, "127.0.0.1", "user_mod", "test", "c_user_module", 3306, NULL, 0) == NULL)
  {
      fprintf(stderr, "%s\n", mysql_error(con));
      mysql_close(con);
      exit(1);
  }

  printf("The connection is open\n");

  mysql_close(con);
  exit(0);
}
```

selanjut save dan sebelum bisa menggunakan aplikasi kita perlu melakukan kompilasi aplikasi terlebih dahulu. Untuk kompilasi aplikasi, kita bisa menggunakan perintah berikut


```
gcc conn.c -o conn `mysql_config --cflags --libs`
```

setelah aplikasi selesai di komplasi, selanjut kita bisa menjalankan aplikasi dengan perintah berikut

```
./conn
```


## Error
Jika terjadi error dalam proses compiling seperti contoh dibawah, 
```
conn.c:(.text+0x15): undefined reference to `mysql_init'
```

berarti anda lupa menhubungkan library mariadbconnector pada saat build aplikasi. berikut ini contoh link library mariadb c connector

```
gcc -o <<OutputFile>> <<SourceFileName>> `mysql_config --cflags --libs`
```

jika kita ingin mencompile file conn.c menjadi aplikasi bernama test, maka kita bisa menggunakan contoh perintah di bawah ini

```
gcc conn.c -o test  `mysql_config --cflags --libs`
```

## Referensi

https://zetcode.com/db/mysqlc/
https://stackoverflow.com/questions/39825428/mysql-error-program-wont-compile-because-of-error/39826687
https://tecadmin.net/docker-compose-persistent-mysql-data/





