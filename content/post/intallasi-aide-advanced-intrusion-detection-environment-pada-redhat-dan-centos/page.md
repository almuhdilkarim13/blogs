---
draft     : false
author    : "Al Muhdil Karim"
title     : "Installasi aide ( Advanced Intrusion Detection Environment ) pada redhat dan centos"
slug      : "setting-ip-statis-menggunakan-perintah-ip-pada-linux"
images    : ""
date      : 2023-12-19
lastmod   : ""
categories: [ DevOps ]
tags      : [ security, aide ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Cara installasi dan konfigurasi aide di linux redhat dan centos"
---

## Installasi

buka terminal dan ketikan perintah berikut

```shell
sudo dnf install aide
```

## Konfigurasi

buat database aide dengan perintah

```shell
sudo aide --init
```

selanjutnya renama database dengan perintah

```shell
sudo mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz 
```

cek apakah aide berjalan dengan perintah

```shell
aide --check
```

jika ouputnya adalah

```shell
Start timestamp: 2023-12-29 04:57:58 +0700 (AIDE 0.16)
AIDE found NO differences between database and filesystem. Looks okay!!

Number of entries:      57924

---------------------------------------------------
The attributes of the (uncompressed) database(s):
---------------------------------------------------

/var/lib/aide/aide.db.gz
  MD5      : g5pyZs2HP/ca48EDEk42EQ==
  SHA1     : MWESfOp0sJKRG2qSy7PdCdFHM+U=
  SHA256   : mXGZj3b1ZTAhUXFMfuvJOaiOFQSdeN6z
             O9qjmvtL/sQ=
  SHA512   : Q9WgJfN/khgjiI0QURPUTUwkjpCrqgLc
             +20hD4SHsO3JEkxz8mi5kcxqrd4ZyoAB
             a4bImW/1phSxCTzpFCaoVA==


End timestamp: 2023-12-29 04:58:11 +0700 (run time: 0m 13s)
```

maka aide telah berhasil terinstall.


