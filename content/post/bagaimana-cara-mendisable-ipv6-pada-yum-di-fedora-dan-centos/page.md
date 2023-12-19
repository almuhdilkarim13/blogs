---
draft     : false
author    : Al Muhdil Karim
title     : "Bagaimana cara mendisable IPV6 pada Yum di Fedora dan Centos"
date      : 2021-04-15
lastmod   : ""
apps      : "yum"
programing: ""
related1 : "yum"
related2 : "fedora"
related3 : "security"
related4 : "DevOps"
categories: [ DevOps ]
tags      : [ IPV6 ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Terkadang perangkat tidak mendukung penggunaan IPV6, Nah bagaimana kita bisa mengatur package manager YUM hanya menggunakan IPV4 pada Fedora dan Centos "
---

Yum adalah package manager yang digunakan oleh Centos. Yum biasanya kita gunakan untuk mempermudah installasi, menghapus atau mengupdate aplikasi dari centos. Dalam pekerjaanya yum biasanya menggunakan dua tipe internet protokol IPV4 dan IPV6. 

![image alt 'Penggunaan protokol internet IPV4 dan IPV6 oleh yum' ](wireshark-screenshoot.webp)

Namun masalahnya, terkadang perangkat dekstop atau server kita tidak selalu berada pada jaringan yang telah mendukung IPV6. Dalam beberapa kasus terjadi error seperti contoh berikut

```sheel
[Errno 14] curl#7 - "Failed to connect to 2404:6800:4005:c00::88: Network is unreachable"
```

Nah dalam kesempatan ini kita akan memabahas bagaimana mendisable IPV6 pada aplikasi yum, sehingga setiap kita menginstall, mengupdate atau menghapus aplikasi pada centos, yum sebagai package manager akan selalu menggunakan IPV4.

Caranya cukup mudah, kita hanya perlu menambahkan `ip_resolve=4` atau `ip_resolve=ipv4` pada file `/etc/yum.conf`.[^1]

Untuk menambahkan hal tersebut, buka terminal dan ketikan perintah berikut

```sheel
echo "ip_resolve=4" >> /etc/yum.conf
```

Setelah itu restart perangkat dengan perintah

```sheel
reboot
```

Sampai disini yum akan menggunakan IPV4 untuk seluruh aktivitas seperti installasi, update ataupun ugprade aplikasi.

[^1]: [ yum(8) — Linux manual page ](https://man7.org/linux/man-pages/man8/yum.8.html).
