---
draft       : false
author      : Al Muhdil Karim
title       : "Memperkuat Keamanan Linux Dengan Menetapkan Masa Kadaluarsa Password"
date       : 2021-06-20
lastmod    : ""
thumbnail  : ""
apps       : "pam"
programing : ""
related1  : "pam"
related2  : "centos"
related3  : "Security"
related4  : "DevOps"
categories : [ DevOps ]
tags       : [ password ]
math       : false
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
    description : "Bagaimana Mempekuat Hash pada PAM untuk mempersulit aktivitas cracking password di linux"
---

Ada beberapa kebiasaan kita dimana kita sangat jarang mengganti password dan bahkan menggunakan satu password untuk berbagai macam akun,

Terkadang menggunakan password yang berbeda untuk setiap akun dan mengganti berkala password, seringkali adalah hal yang melelahkan,

Akan tetapi, jika anda mempunyai perhatian terhadap keamanan digital, anda pasti akan memahami hal tersebu pada dasarnya sangat berisiko.

Untuk meminimalisir hal tersebut, anda cukup memberikan batasan berupa masa berlaku password

Untuk hal tersebut cukup mudah, anda tinggal mengetikan perintah berikut diterminal

```
chage -M 365 root
```

**Berikut penjelasanya**

`365` adalah waktu berlakunya password ( 365 Hari / 1 tahun )

`root` adalah nama username dari user

Jika anda mempunyai user dengan username `johndoe`  dan ingin memberikan masa berlaku password selama `dua tahun` maka anda bisa menggunakan perintah 

```
chage -M 730 johndoe
```

`730` adalah waktu berlakunya password ( 365 Hari * 2 =  2 tahun )

`johndoe` adalah nama username dari user

Untuk penggunaan komputer desktop tentu ini bukanlah hal yang terlalu prinsipil, namun saat anda seorang sistem administrator, tentu ini akan sangat membantu dalam manajemen akses terhadao suatu akun pada sistem yang anda kelola.
