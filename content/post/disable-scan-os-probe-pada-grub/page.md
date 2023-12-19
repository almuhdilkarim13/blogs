---
draft     : false
author    : "Al Muhdil Karim"
title     : "Menokatifkan grub untuk list os lainya pada linux"
slug      : "disable-scan-os-prober-pada-grub"
images    : ""
date      : 2023-12-19
lastmod   : ""
categories: [ Desktop ]
tags      : [ linux, grub2 ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Cara menonaktivkan fitur scan os dan list os lainya pada grub di linux."

---



## Modifikasi file grub

backup file `/etc/default/grub` dengan perintah berikut

```shell
cp /etc/default/grub /etc/default/grub.origin
```

lalu modifikasi file dengan perintah

```shell
echo /etc/default/grub
```

atau

```shell
nano /etc/default/grub
```



lalu cari field berikut ini pada file tersebut 

```shell
GRUB_DISABLE_OS_PROBER=false
```

ganti menjadi konfigurasi menjadi 

```shell
GRUB_DISABLE_OS_PROBER=true
```



jika `GRUB_DISABLE_OS_PROBER` silahkan di tambahkan sendiri kedalam file tersebut dan tempatkan di barisan paling bawah.



## Update File Grub

Setelah melakukan perubahan konfigurasi, maka update grub

untuk distro linux berbasis .deb seperti debian, ubuntu, kali linux gunakan perintah berikut ini

```shell
sudo update-grub
```

sedangkan untuk distro berbasis RedHat gunakan perintah berikut ini.

```shell
sudo grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
```

tunggu sampai proses selesai dan kemudian hidupkan ulang komputer dengan mengetikan perintah



```shell
reboot
```




