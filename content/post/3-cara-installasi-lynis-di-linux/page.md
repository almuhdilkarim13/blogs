---
draft     : false
author    : "Al Muhdil Karim"
title     : "3 Cara Installasi Lynis Di Linux"
slug      : "3-cara-installasi-lynis-di-Linux"
images    : helena-hertz-wWZzXlDpMog-unsplash.jpg"
date      : 2021-05-24
lastmod   : ""
categories: [ DevOps ]
tags      : [ Audit, lynis ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Menginstall lynis dengan mudah dengan berbaagai metode dan untuk berbagai distro linux "
---

Lynis adalah salah satu software yang biasa digunakan untuk melakukan audit keamanan
sistem operasi berbasis linux dan Macos. Software ini dikembangkan oleh Cisofy dan mempunyai 
dua versi, lynis (open soure ) dan lynis enterprise (berbayar). 

Dalam tutorial ini, kita akan memasang lynis yang menggunakan lisensi open source. 
Untuk memasang lynis pada sistem linux, setidaknya ada 3 metode pemasangan yang bisa kita gunakan

## Metode 1 - Menggunakan Package Management (Recommended)

### Package Management Berbasis RPM

Berikut ini adalah tabel dari beberapa distro linux berbasis RPM yang telah menyediakan lynis didalam
repository aplikasi mereka.

| No  | Nama Distro  |
| --- | ------------ |
| 1   | Linux edora  |
| 2   | Linux Centos |
| 3   | Linux RedHat |

Untuk distro di atas, kita bisa menginstall lynis dengan membuka terminal.
lalu mengetikan perintah di bawah ini kedalam terminal.

```shell
sudo yum install lynis
```

### Package Management Berbasis DEB

Jika anda termasuk pengguna distro yang termasuk didalam tabel di bawah ni, 
berarti anda menggunakan linux dengan package manajemen berbasis DEB.

| No  | Nama Distro  |
| --- | ------------ |
| 1   | Linux Debian |
| 2   | Linux Ubuntu |
| 3   | Linux Mint   |

Anda dapat memulai installasi, dengan membuka terminal dan mengetikan perintah
dibawah ini kedalam terminal anda.

```shell
sudo apt-get install lynis
```

### openSUSE

Bagi anda yang menggunakan sistem operasi Open Suse, 
anda dapat mengetikan perintah dibawah ini didalam terminal anda

```shell
sudo zypper install lynis
```

### Arch Linux

Sedangkan untuk pengguna distro berbasis arch, seperti

| No  | Nama Distro   |
| --- | ------------- |
| 1   | Arch Linux    |
| 2   | Manjaro Linux |
| 3   | Garuda LInux  |

anda bisa mengetikan perintah berikut ini kedalam terminal,

```shell
sudo pacman -S lynis
```

#### Memulai audit

Untuk melakukan audit, ketikan perintah di bawah ini kedalam terminal

```shell
lynis audit system
```

## Metode 2 - Menggunakan Git

Pertama sekali, kita harus memastikan aplikasi git telah terpasang. 
Untuk hal tersebut kita bisa mengetikan perintah

```shell
git version
```

berikut contoh output dari perintah diatas

```shell
[karim@minigun ~]$ git version
    git version 2.31.1
```

Berdasarkan output, diketahui kalau saya menggunakan aplikasi git
dengan versi 2.31.1. Dari informasi tersebut berarti linux yang saya
gunakan telah terpasang aplikasi git sebelumnya. Jika belum terpasang
silahkan install aplikasi git terlebih dahulu.

setelah itu, kita harus masuk kedalam direktori dimana kita akan 
melakukan pemasangan. Saya akan memasang aplikasi didalam direktori

```shell
/usr/local
```

 jadi untuk masuk ke direktori, saya akan mengetikan perintah

```shell
 cd /usr/local
```

Selanjutnya kita akan mengcloning source code dari git repository 
keadalam perangkat. ketikan perintah berikut kedalam terminal

```shell
git clone https://github.com/CISOfy/lynis
```

berikut ini adalah ouput dari perintah tersebut.

```shell
[karim@minigun ~]$ git clone https://github.com/CISOfy/lynis
    Cloning into 'lynis'...
    remote: Counting objects: 1733, done.
    remote: Compressing objects: 100% (8/8), done.
    remote: Total 1733 (delta 3), reused 0 (delta 0), pack-reused 1725
    Receiving objects: 100% (1733/1733), 886.18 KiB | 378.00 KiB/s, done.
    Resolving deltas: 100% (1204/1204), done.
    Checking connectivity... done.
```

#### Memulai audit

Untuk melakukan audit, kita harus masuk kedalam direktori 
intallasi lynis terlebih dahulu. Ketikan perintah di bawah ini 

```shell
cd lynis
```

setelah itu baru ketikan perintah

```shell
lynis audit system
```

## Metode 3 - Menggunakan Tarball file

Metode ini digunakan jika distro linux yang anda gunakan tidak 
memberikan dukungan untuk kedua cara sebelumnya. 

Dalam tutorial ini saya akan menggunakan direktori `/usr/local` sebagai lokasi installasi.
Pertama sekali kita harus membuat folder installasi terlebih dahulu, untuk itu kita akan menggetikan perintah

```shell
mkdir /usr/local/lynis
```

lalu masuk kedalam folder installasi dengan perintah

```shell
cd /usr/local/lynis
```

selanjutnya kita perlu mengunduh file tarball dari lynis kedalam folder installasi,
dengan mengunjungi alamat berikut [Lynis Release](https://cisofy.com/downloads/lynis/). 

copy link dari tombol download yang terdapat didalam halaman tersebut dengan mengklik kanan pada tombol, lalu pilih menu 'copy link address'.
untuk mendonwload kita bisa menggunakan perintah wget atau curl. berikut contohnya

```shell
sudo wget https://downloads.cisofy.com/lynis/lynis-3.0.3.tar.gz
```

atau

``` shell
sudo curl https://downloads.cisofy.com/lynis/lynis-3.0.3.tar.gz -o lynis-3.0.3.tar.gz
```

Setelah unduhan selesai, kita harus untuk mengekstrak file yang diunduh terlebih dahulu. 
untuk mengekstrak file, bisa menggunakan perintah di bawah ini

```shell
tar xfvz lynis-3.0.3.tar.gz
```

#### Memulai audit

Untuk melakukan audit, kita bisa mengetikan perintah berikut

```shell
sudo ./lynis audit system
```
