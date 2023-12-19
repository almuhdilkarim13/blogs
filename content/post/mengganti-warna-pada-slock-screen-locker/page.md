---
draft     : false
author    : Al Muhdil Karim
title     : Cara Mengganti Warna Default Pada Slock Screen Locker 
date      : 2021-04-21
lastmod   : ""
thumbnail : ""
apps       : "slock"
programing : ""
related1 : "slock"
related2 : "arch"
related3 : "security"
related4 : "DevOps"
categories: [ Desktop ]
tags      : [ archlinux ]
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
    description : "Cara mengganti warna pada slock screen locker harus dilakukan secara manual, karena slock tidak memberikan opsi file konfigurasi ketika telah di install."
---

Cara mengganti warna pada slock adalah dengan melakukan kompilasi ulang terhadap aplikasi slock secara manual. Kondisi ini memang agak sedikit mengecewakan, namun tidak ada salahnya untuk di coba, mungkin untuk menambah pengetahuan kita dalam menggunakan linux. 

## Uninstall Slock ( Optional )

Jika anda telah menginstall slock dengan mengikuti artikel "[ini](https://almuhdilkarim.com/blog/mengenal-slock-simple-screen-saver-linux-yang-berorientasi-kepada-performa-dan-keamanan/)" atau anda telah menginstall `slock` meggunakan package manager dari distribusi linux anda, maka anda harus menghapus `slock` terlebih dahulu.

### Arch Linux

* Buka terminal emulator

* Ketikan perintah di bawah ini [^1]
  
  ```shell
  sudo pacman -Rs slock
  ```

* Kemudian tunggu sampai proses uninstall selesai

### Linux Ubuntu

* Buka terminal emulator

* Ketikan perintah di bawah ini
  
  ```shell
  sudo apt-get remove slock
  ```

* Kemudian tunggu sampai proses uninstall selesai

### Linux Fedora

* Buka terminal emulator

* Ketikan perintah di bawah ini
  
  ```shell
  sudo yum remove slock
  ```

* Kemudian tunggu sampai proses uninstall selesai

## Mempersiapkan Source Code

Sebelum melakukan kompilasi, kita harus mempersiapkan terlebih dahulu source code dari aplikasi `slock`. Source ini tersedia di github, langkah yang harus kita lakukan adalah memilih direktori yang akan menampung source code `slock`, dalam tutorial ini saya menggunakan direktori `tmp`. 

Alasan saya menggunakan direktori ini karena setiap data yang ada didalam direktori ini akan terhapus secara otomatis ketika perangkat dimatikan, anda bebas menggunakan direktori yang anda sukai untuk proses ini. 

Untuk masuk kedalam direktori `/tmp`  anda bis ketikan perintah dibawah ini 

```shell
cd /tmp
```

Langkah selanjutnya, kita akan mengunduh source code slock. Untuk mengunduh source code dari `slock` anda bisa menggunakan dua pendekatan, menggunakan git atau curl.

### Opsi 1 - Menggunakan Git

Untuk git, ketikan  perintah berikut untuk mengcloning source code `slock` 

```
git clone git://git.suckless.org/slock
```

setelah proses cloning selesai, masuk kedalam directory slock dengan perintah

```
cd /tmp/slock
```

Jika anda belum memasang aplikasi git didalam linux anda, anda bisa mengunduh slock dengan menggunakan perintah curl.

### Opsi 2 - Menggunakan Curl

Utuk mengunduh source code `slock` menggunakan curl, silahkan ketikan perintah dibawah ini

```shell
curl -O https://dl.suckless.org/tools/slock-1.4.tar.gz slock.tar.gz
```

setelah itu kita harus mengekstrak file terlebih dahulu. Untuk  ekstrasi ketikan perintah dibawah ini pada terminal

```shell
tar -xf slock-1.4.tar.gz
```

selanjutnya baru kita masuk kedalam direktori slock dengan perintah

```shell
cd slock-1.4
```

## Merubah File Konfigurasi

Setelah mempersiapkan source code, kita harus megubah file konfigurasi dari `slock` untuk bisa mengubah warna default dari `slock` . Jadi nantinya aplikasi akan menggunakan warna yang kita pilih ketika menjalankan fungsinya.

Untuk merubah warna default tersebut kita harus mengedit file yang bernama `config.def.h`.[^2] Untuk mengubah file tersebut, kita bisa menggunakan perintah berikut,

```
nano config.def.h
```

Output dari perintah tersebut adalah sebagai berikut

```shell
/* user and group to drop privileges to */
static const char *user  = "nobody";
static const char *group = "nogroup";

static const char *colorname[NUMCOLS] = {
        [INIT] =   "black",     /* after initialization */
        [INPUT] =  "#005577",   /* during input */
        [FAILED] = "#CC3333",   /* wrong password */
};

/* treat a cleared input like a wrong password (color) */
static const int failonclear = 1;
```

Hal yang pertama kali harus kita lakukan adalah mengedit baris `static const char *user`dan `static const char *group` yang ada di dalam output di diatas.  G

Kita akan mengganti  `nobody`  dan  `nogroup` dengan username dan group dari akun linux kita. Jika anda ingin memastikan username dari akun linux anda, anda bisa mengetikan perintah berikut di terminal

```shell
whoami
```

Output dari perintah tersebut adalah sebagai berikut

```shell
[karim@minigun ~]$ whoami
karim
```

dari perintah tersebut dapat diketahui kalau saya menggunakan user karim untuk login, sehingga saya akan menggnati `nobody` dan `nogroup` menjadi `karim`.  

Berikut file cofigurasi saya, setelah menyunting user account yang akan digunakan.

```shell
/* user and group to drop privilegess to */
static const char *user  = "karim";
static const char *group = "karim";

static const char *colorname[NUMCOLS] = {
        [INIT] =   "black",     /* after initialization */
        [INPUT] =  "#005577",   /* during input */
        [FAILED] = "#CC3333",   /* wrong password */
};

/* treat a cleared input like a wrong password (color) */
static const int failonclear = 1;
```

Setelah mengganti user, baru kita akan mengganti warna default dari slock. Untuk mengganti warna default kita akan megubah nilai pada bagian ` [INIT]`, ` [INPUT] `, `[FAILED]`.

* [INIT] adalah background default yang berwarna hitam
* [INPUT] adalah background saat kita sedang menginput password dan berwarna biru
* [FAILED] adalah background saat password yang kita input salah,warnanya adalah merah.

Dalam mengganti warna kita bisa menggunakan nama warna atau kode warna berbasis hex. Pada percobaan ini saya akan menggnati warna baris ` [INPUT] ` dan `[FAILED]` yang awalnya berwarna biru dan merah menjadi warna hijau dan orange. 

Berikut file konfigurasi akhir saya setelah saya mengganti warna

```shell
/* user and group to drop privileges to */
static const char *user  = "karim";
static const char *group = "karim";

static const char *colorname[NUMCOLS] = {
        [INIT] =   "black",     /* after initialization */
        [INPUT] =  "#3DCC33",   /* during input - telah diganti */
        [FAILED] = "#FFA500",   /* wrong password - telah diganti */
};

/* treat a cleared input like a wrong password (color) */
static const int failonclear = 1;
```

Keterangan :

* #3DCC33 adalah kode hex untuk warna Hijau
* #FFA500 adalah kode hex untuk warna Orange

## INSTALLASI

Untuk bisa menggunakan slock dengan konfigurasi yang telah kita sesuikan, kita harus mengcompile source code tersebut terlebih dahulu. Untuk kompilasi source code kita bisa menggunakan perintah berikut.

```shell
make
```

setelah proses `make` selesai, kita bisa memulai proses installasi dengan mengetikan perintah

```shell
sudo make install
```

tunggu sampai proses installasi sampai selesai. 

Untuk mulai menggunakan slock,  kita bisa mengetikan perintah berikut di terminal

```sh
slock
```

Sampai di sini, ketika kita telah berhasil mengganti warna pada slock.  Ketika kita mengetikan password , warna layar yang awalnya berwarna biru akan berubah menjadi hijau. Begitu juga ketika kita salah mengetikan password, yang awalnya berwarna merah akan berganti menjadi orange.

[^1] [Arch Wiki](https://wiki.archlinux.org/index.php/Slock)
[^2] [Suckless.org](https://tools.suckless.org/slock/)
