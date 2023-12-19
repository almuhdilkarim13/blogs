---
draft      : false
author     : Al Muhdil Karim
title      : "Installasi Flutter dan Android Studio Pada Arch Linux "
date       : 2022-01-09
lastmod    : ""
apps       : ""
programing : "c"
related1   : "flutter"
related2   : "android-studio"
related3   : "arch-linux"
related4   : "Development"
categories: [ Programing ]
tags      : [ ide , flutter ]
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
    description : "Terkadang perangkat tidak mendukung penggunaan IPV6, Nah bagaimana kita bisa mengatur package manager YUM hanya menggunakan IPV4 pada Fedora dan Centos"
---

## Optional - Installasi Yay

Flutter dan android studio berada dalam paket komunitas pada arch linux. untuk mempermudah installasi dan manajamen aplikasi ada baiknya kita install terlebih dahulu package management,dalam hal ini saya menggunakan Yay. Untuk menginstall yay, kita membutuhkan `git` dan `base-devel`, untuk menginstall kedua paket tersebut kita bisa menggunakan perintah berikut:

```
sudo pacman -S --needed git base-devel
```
Setelah itu kita mempersiapkan direktori yang akan kita gunakan untuk menampung file aplikas, biasanya saya menggunakan direktori `tmp` untuk menampung file tersebut, karena direktori `tmp` akan di hapus secara otomatis saat system di matikan. untuk langkah selanjutnya, kita masuk kedalam direktori `tmp` dengan menggunakan perintah berikut

```
cd /tmp
```
selanjutnya kita akan mengcloning file aplikasi yay dari github dengan menggunakan perintah berikut
```
git clone https://aur.archlinux.org/yay.git
```
setelah proses cloning selesai, langkah selanjutnya kita masuk kedalam direktori yay yang barusan kita download dengan menggunakan perintah berikut:
```
cd yay
```
setelah semua proses diatas, langkah terakhirnya adalah menginstall aplikasi yay dengan menggunakan perintah berikut:
```
makepkg -si
```
Sampai di sini kita telah berhasil menginstall aplikasi yay, selanjutnya kita akan memulai proses installasi flutter dan android studio.

## Installasi java

Untuk bisa berjalan dengan baik, flutter membutuhkan java dengan versi 8 atau 10. Untuk memastikan java yang terinstall dalam arch linux, kita bisa menggunakan perintah berikut :
```
java -version
```

jika output dari perintah tersebut menampikan pesan error atau not found, kita perlu untuk menginstall java terlebih dahulu pada arch linux, silahkan gunakan perintah berikut untuk memasangkan java pada arch linux

```
sudo pacman -S jre8-openjdk
```

setelah proses installasi selesai, kita perlu menambahkan environtment path kedalam file `.bashrc` atau `zshrc`, tergantung shell mana yang kita gunakan. karena saya menggunakan bash, maka saya akan memodifikasi file `.bashrc` dengan perintah berikut:

```
nano $HOME/.bashrc
```

Selanjutnya tambahkan beberapa baris di bawah ini pada file `.bashrc`

```
export JAVA_HOME='/usr/lib/jvm/java-8-openjdk'
export PATH=$JAVA_HOME/bin:$PATH 
```

## Installasi Flutter

untuk menginstall flutter, silahkan ketikan perintah berikut di terminal
```
yay -S flutter
```

yay akan menginstall flutter pada direktori `/opt/flutter`, direktori ini pada dasarnya hanya bisa di akses oleh administratif user (red - root). jadi kita perlu meberikan akses tambahan untuk direktori flutter kepada user yang sedang kita gunakan. 

pertama sekali, kita perlu menambahkan group flutter dengan perintah :
```
sudo groupadd flutterusers
```
selanjutnya kita mendaftarkan user yang sedang kita gunakan dengan perintah
```
sudo gpasswd -a $USER flutterusers
```
setelah itu kita tinggal menabahkan hak akses user pada direktori `/opt/flutter` dengan menggunakan perintah berikut :
```
sudo chown -R :flutterusers /opt/flutter
```
dan
```
sudo chmod -R g+w /opt/flutter/
```

jika anda menemukan error, maka anda bisa meggunakan perintah berikut:

```
sudo chown -R $USER /opt/flutter
```
## Install android sdk dan alat lainya

Untuk installasi android sdk, ketikan perintah berikut di terminal
```
yay -S android-sdk android-sdk-platform-tools android-sdk-build-tools
```
selannjutnya
```
yay -S android-platform
```
setelah itu kita harus mengkonfigurasikan user permission untuk meggunakan android sdk. pertama 
pertama sekali, kita perlu menambahkan group untuk sdk dengan perintah :
```
sudo groupadd android-sdk
```
selanjutnya kita mendaftarkan user yang sedang kita gunakan dengan perintah
```
sudo gpasswd -a $USER android-sdk
```
setelah itu kita tinggal menabahkan hak akses user pada android sdk dengan menggunakan perintah berikut :
```
sudo setfacl -R -m g:android-sdk:rwx /opt/android-sdk
```
dan
```
sudo setfacl -d -m g:android-sdk:rwX /opt/android-sdk
```
langkah terkahir adalah menambahkan environtment path android sdk kedalam file `.bashrc` atau `.zshrc`, tergantung shell mana yang anda gunakan. Untuk menbahkannya ketikan perintah berikut di terminal
```
nano $HOME/.bashrc
```

selanjutnya copy perintah berikut kedalam file `.bashrc`

```
## sdk
#SDK exporting - this will solve your issue
export ANDROID_HOME=$HOME/Android/Sdk

#Tools exporting - it can be need in your case
export PATH=$HOME/Android/Sdk/platform-tools:$PATH
export PATH=$HOME/Android/Sdk/tools:$PATH
export PATH=$HOME/Android/ndk-build:$PATH
```

## Install android studio

Setelah menginstall flutter kita bisa menginstall android studio dengan perintah berikut
```
yay -S android-studio
```
tunggu sampai installasi selesai, setelah itu buka android studio, ikuti langkah selanjutnya, untuk pertamakalinya anda akan di minta untuk mengunduh beberapa package. 

## Finishing

Sebelum memulai pengembangan kita perlu menyetujui license dari android terlebih dahulu. Ketikan perintah ini di terminal :
```
flutter doctor --android-licenses
```

setelah itu kita bisa menjalankan verifikasi terhadap installasi flutter dengan menggunakan perintah
```
flutter doctor
```

jika anda menemukan error seperti berikut

```
! Cannot find chrome . Try setting CHROME_EXECUTABLE to a chrome excutable
```

maka kita perlu menabahkan path envrontmen dari google chrome dengan file `.bashrc` atau `.zhsrc`, 
ketikan perintah
```
nano $HOME/.bashrc
```
dan tambahkan baris berikut pada akhir file
```
## chrome
export CHROME_EXECUTABLE=/opt/google/chrome/google-chrome
```

selanjutnya anda bisa mencoba kembali untuk melakukan verifikasi dengan perintah:

```
flutter doctor
```




