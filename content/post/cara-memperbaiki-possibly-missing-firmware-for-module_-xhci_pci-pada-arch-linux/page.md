---
draft      : false
author     : Al Muhdil Karim
title      : "Memperbaiki Possibly Missing Firmware for Module Xhci_pci pada Arch Linux"
date       : 2021-06-16
lastmod    : ""
apps       : "firmware"
programing : ""
related1   : "firmware"
related2   : "arch"
related3   : "Kernel"
related4   : "DevOps"
categories : [ Desktop ]
tags       : [ archlinux, firmware ]
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
    description : "Tutorial ini membahas bagaimana memperbaiki Possibly Missing Firmware for Module Xhci_pci"
---

Buka terminal dan masuk kedalam direktori `/tmp` dengan mengetikan perintah 

```shell
 cd /tmp
```

selanjut download file firmware dari github

```shell
git clone https://aur.archlinux.org/upd72020x-fw.git
```

buka file yang telah di download dengan perintah

```shell
cd upd72020x-fw
```

Install firmware menggunakan perintah 

```shell
makepkg -sri
```

Langkah terakhir adalah update mkinitcpio

```shell
sudo mkinitcpio -p linux
```

Restart

```shell
systemd reboot
```
