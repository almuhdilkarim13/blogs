---
draft      : false
author     : Al Muhdil Karim
title     : "Cara Installasi Iptables Pada Centos 7"
date      : 2021-04-17
lastmod   : ""
thumbnail : ""
apps      : "iptables"
programing: ""
related1 : "iptables"
related2 : "centos"
related3 : "security"
related4 : "DevOps"
categories: [ DevOps ]
tags      : [ Firewall, Iptables ]
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
    description :  "Dalam tutorial ini kita akan membahas bagaimana melakukan installasi iptables pada centos 7"
---

## Persiapan

Untuk menginstall iptables pada centos 7, pertama kita harus menghapus firewalld yang telah terpasang secara default. Hal ini ditujukan untuk menghindari konflik antara iptables dan firewalld. 

Untuk menghapus firewalld, pertama kita harus menghentikan service firewalld dengan mengetikan perintah

```sheel
sudo systemctl stop firewalld
```

Selanjutnya kita mendisable service firewalld saat proses booting. Untuk mendisable service firewalld saat booting ketikan perintah berikut

```sheel
sudo systemctl disable firewalld
```

Setalah mendisable service firewalld saat proses booting, kita bisa memastikan service tersebut tidak di load oleh system dengan mengetikan perintah berikut ini

```sheel
sudo systemctl mask --now firewalld
```

Sampai di sini kita telah siap utuk memulai proses installasi iptables

## Installasi

Untuk menginstall iptables, ketikan perintah berikut di terminal [^1]

```sheel
sudo yum install iptables-services
```

Setelah proses intallasi selesai, kita harus mengaktivkan service iptables baik untuk IPV4 atau IPV6.Jika sistem jaringan tidak menggunakan IPV6 maka cukup untuk mengaktivkan iptables untuk IPV4.

***Mengaktivkan iptables untuk IPV4***

```sheel
sudo systemctl start iptables
```

***Mengaktvikan iptables untuk IPV6***

```sheel
sudo systemctl start ip6tables
```

## Autostart iptables Pada saat booting

Kalau ingin mengaktivkan iptables secara otomatis pada saat booting, kita harus mengenable terlebih dahulu dengan perintah berikut

***Autostart iptables untuk IPV4***

```sheel
sudo systemctl enable iptables
```

***Autostart iptables untuk IPV6***

```sheel
sudo systemctl enable ip6tables
```

## Mengecek status iptables

Dalam beberapa kasus, terkadang kita ingin melihat apakah iptables telah aktiv pada sistem kita atau belum. Cara untuk mengetahui status tersebut adalah dengan mengetikan perintah berikut

***Mengecek service iptables untuk IPV4***

```sheel
sudo systemctl status iptables
```

***Mengecek service iptables untuk IPV6***

```sheel
sudo systemctl status ip6tables
```

## Cara melihat rules network dari iptables

Sebelum melakukan konfigurasi, terkadang kita lupa terkait konfigurasi aturan pada firewall yang telah di konfigurasi sebelumnya. Nah untuk mengecek aturan network yang telah di jalankan oleh iptables, kita bisa menggunakan perintah berikut

***Membaca rules iptables IPV4***

```sheel
sudo iptables -nvL
```

***Membaca rules iptables IPV6***

```sheel
sudo ip6tables -nvL
```

atau jika anda ingin melihat sekaligus merubah konfigurasi, maka bisa langsung dilakukan dengan mengubah file pada direktori `/etc/sysconfig/iptables` untuk IPV4 dan `/etc/sysconfig/ip6tables` untuk IPV6.

untuk merubah file konfigurasi anda bisa menggunakan perintah berikut menggunakan nano atau editor kesayangan anda lainya.

***IPV4***

```sheel
    sudo nano /etc/sysconfig/iptables
```

***IPV6***

```sheel
    sudo nano /etc/sysconfig/ip6tables
```

Jika output perintah tersebut adalah `command not found`, silahkan pasang terlebih dahulu nano editor dengan perintah

```sheel
 sudo yum install nano
```


[^1]: [Arch Wiki : Iptables](https://wiki.archlinux.org/title/Iptables)
