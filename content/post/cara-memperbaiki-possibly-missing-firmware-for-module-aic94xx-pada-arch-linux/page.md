---
draft     : false
author    : Al Muhdil Karim
title     : "Memperbaiki Possibly Missing Firmware for Module: Aic94xx Pada Arch Linux"
date      : 2021-06-12
lastmod    : ""
thumbnail  : ""
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
    description : "Tutorial ini membahas bagaimana memperbaiki Possibly Missing Firmware for Module Aic94xx"
---

Buka terminal

Masuk kedalam folder `/tmp`, dengan mengetikan perintah berikut di terminal

```shell
cd /tmp
```

Clone file firmware dari github

```shell
git clone https://aur.archlinux.org/aic94xx-firmware.git
```

Masuk kedalam folder firmware

```shell
cd aic94xx-firmware
```

Install firmware 

```shell
makepkg -sri
```

Update konfigurasi mkinitcpio

```shell
sudo mkinitcpio -p linux
```

Restart perangkat dengan perintah

```shell
systemd reboot
```
