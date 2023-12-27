---
draft: false
title: Memperkuat Otentikasi Pada Linux Dengan Meningkatkan Hashing Rounds PAM
date: 2021-06-18
lastmod: ""
apps: pam
programing: ""
related1: pam
related2: centos
related3: security
related4: DevOps
categories:
  - DevOps
tags:
  - pam
math: false
article:
  status: true
  thumbnail: ""
  teaser: ""
slide:
  status: false
  event: ""
  organize: ""
audio:
  status: false
  url: ""
  sub_id: ""
  sub_en: ""
  desc: ""
video:
  status: false
  videoid: ""
  desc: ""
seo:
  index: ""
  nofollow: ""
  keyword: ""
  cover: ""
  description: Bagaimana Mempekuat Hash pada PAM untuk mempersulit aktivitas cracking password di linux
---

## PAM dan Password Cracking

Cracking password adalah salah satu metode klasik dalam mendapatkan credential dari suatu sistem linux, baik itu menggunakan bruteforce atau menggunakan dictionary attack.

PAM sendiri adalah modul yang bertanggung jawab dalam proses otentikasi pengguna bagi banyak distro linux. Dalam kesempatan ini, kita akan belajar bagaimana meningkatkan hashing rounds pada password pada PAM.

Diharapkan dengan meningkatkan hashing round, maka password kita akan lebih sulit untuk di dekripsi sehingga berdampak kepada meningkatnya sistem keamanan pada sistem operasi berbasis linux.

Untuk meningkatkan hashing rounds kita hanya perlu merubah konfigurasi PAM linux. Yang perlu kita lakukan adalah memodifikasi file `password-auth` dan file `system-auth`

## Memperkuat Otentikasi Password ( password-auth )

Konfigurasi `password-auth` dapat anda temukan pada direktori

```shell
/etc/pam.d/password-auth
```

Untuk menambahkan hashing rounds, edit file tersebut dengan perintah

```shell
nano /etc/pam.d/password-auth
```

Selanjutnya cari baris yang bersikan sintaks berikut

```shell
password sufficient pam_unix.so try_first_pass use_authtok nullok sha512 shadow
```

selanjutnya tambahkan `rounds=50000` pada bagian belakang baris tersebut, sehingga sintaks sebelumnya menjadi seperti berikut

```shell
password sufficient pam_unix.so try_first_pass use_authtok nullok sha512 shadow rounds=50000
```

`rounds=50000` bisa anda ganti sesuai selera, bisa menjadi `rounds=30000` atau yang lainya. akan tetapi direkomendasikan menggunakan rentang 20000-50000,

## Memperkuat Otentikasi system ( system-auth )

Konfigurasi `system-auth` dapat anda temukan pada direktori

```shell
`/etc/pam.d/system-auth`
```

Untuk menambahkan hashing rounds, edit file tersebut dengan perintah

```shell
nano /etc/pam.d/system-auth
```

Selanjutnya cari baris yang bersikan sintaks berikut

```shell
password sufficient pam_unix.so try_first_pass use_authtok nullok sha512 shadow
```

Selanjutnya tambahkan `rounds=50000` pada bagian belakang baris tersebut, sehingga sintaks sebelumnya menjadi seperti berikut

```shell
password sufficient pam_unix.so try_first_pass use_authtok nullok sha512 shadow rounds=50000
```

Sama seperti sebelumnya, anda bisa mengganti `rounds=50000` sesuai dengan keinginan anda.
