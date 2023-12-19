---
draft     : false
author    : Al Muhdil Karim
title     : "Memperbaiki Possibly Missing Firmware for Module: Wd719x Pada Arch Linux"
date      : 2021-05-17
lastmod   : ""
thumbnail : ""
apps       : "firmware"
programing : ""
related1 : "firmware"
related2 : "arch"
related3 : "Kernel"
related4 : "DevOps"
categories: [ Desktop ]
tags      : [ archlinux, firmware ]
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
    description : "Tutorial ini membahas bagaimana memperbaiki Possibly Missing Firmware for Module Wd719x"
---

Buka terminal dan masuk kedalam folder `/tmp`, dengan mengetikan perintah berikut

```shell
cd /tmp
```

download file firmware dari github

```shell
git clone https://aur.archlinux.org/wd719x-firmware.git
```

Masuk kedalam direktori dari file firmware

```shell
cd wd719x-firmware
```

Mulai proses install firmware dengan perintah 

```shell
makepkg -sri
```

Update konfigurasi mkinitcpio 

```shell
sudo mkinitcpio -p linux
```

Setelah itu restart linux dengan perintah

```shell
systemd reboot
```
