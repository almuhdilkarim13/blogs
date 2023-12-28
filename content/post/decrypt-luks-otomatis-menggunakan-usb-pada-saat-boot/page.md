---
draft     : false
author    : "Al Muhdil Karim"
title     : "Decrypt lusk volume otomatis menggunakan usb flashdik pada saat boot"
slug      : "decrypt-luks-otomatis-menggunakan-usb-pada-saat-boot"
images    : ""
date      : 2023-12-28
lastmod   : ""
categories: [ DevOps ]
tags      : [ crypto, luks2 ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Cara melakukan decrypt otomatis pada volume root pada saat boot sehingga tidak usah memasukan password secara manual."
---

## Persiapan

Dalam persiapan ini kita akan mebuat sebuah partisi ESP dengan format fat16 pada usb. Tipe partisi ESP dengan format FAT 16 ditujukan untuk menghindari paritisi secara otomatis terbaca saat di gunakan. Untuk operasi ini, kita membutuhkan akses root, oleh sebab itu kita mengganti terlebih dahulu roles menjadi root dengan perintah:

```shell
sudo su
```

Selanjutnya kita mencari path dari usb yang akan kita gunakan dengan perintah:

```shell
lsblk 
```

Berikut contoh output dari perintah tersebut:

```shell
NAME                             MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda                                8:0    1   7.3G  0 disk  
├─sda1                             8:1    1   1.9G  0 part  
└─sda2                             8:2    1   5.4G  0 part  
  └─luks-abc8e9f3-1186-4ef3-b2f0-3ed275e07365
                                 253:5    0   5.4G  0 crypt /run/media/null/datums
nvme0n1                          259:0    0 476.9G  0 disk  
├─nvme0n1p1                      259:1    0   260M  0 part  /boot/efi
├─nvme0n1p2                      259:2    0    16M  0 part  
├─nvme0n1p3                      259:3    0 250.9G  0 part  
├─nvme0n1p4                      259:4    0   1.1G  0 part  
├─nvme0n1p5                      259:5    0     1G  0 part  /boot
└─nvme0n1p6                      259:6    0 223.6G  0 part  
  └─luks-36ea0348-6860-4926-b799-9dca2f7244eb
                                 253:0    0 223.6G  0 crypt 
    ├─rhel_karim-root            253:1    0    30G  0 lvm   /
    ├─rhel_karim-swap            253:2    0   7.7G  0 lvm   [SWAP]
    ├─rhel_karim-home            253:3    0 115.9G  0 lvm   /home
    └─rhel_karim-var_lib_libvirt 253:4    0    70G  0 lvm   /var/lib/libvirt
```

kita akan menggunakan `sda1` sebagai paritisi yang berada didalam usb drive untuk diubah menjadi EPS dengan format FAT16, kita akan menggunakan cfdisk untuk menggati tipe partisi dengan perintah:

```shell
cfdisk /dev/sda
```

output dari perintah tersebut adalah:

```shell
                              Disk: /dev/sda
              Size: 7.26 GiB, 7798784000 bytes, 15232000 sectors
         Label: gpt, identifier: B81DEE59-E012-4A1F-ADED-E8B1C110177D

    Device               Start          End      Sectors     Size Type
>>  /dev/sda1             2048      3909631      3907584     1.9G EFI System   
    /dev/sda2          3909632     15230975     11321344     5.4G unknown






 ┌───────────────────────────────────────────────────────────────────────────┐
 │ Partition UUID: BD619216-97B6-4245-9E0B-0DD7E121F602                      │
 │ Partition type: EFI System (C12A7328-F81F-11D2-BA4B-00A0C93EC93B)         │
 │Filesystem UUID: 38CD-98E8                                                 │
 │     Filesystem: vfat                                                      │
 └───────────────────────────────────────────────────────────────────────────┘
    [ Delete ]  [ Resize ]  [  Quit  ]  [  Type  ]  [  Help  ]  [  Write ]
    [  Dump  ]

                     Quit program without writing changes
```

pilih drive dan kemudian ambil menu `type` pada menu bagian bawah. selanjutnya pilih `EFI System` seperti contoh dibawah ini:

```shell
                       ┌ Select partition type ───────┐
                       │ EFI System                   │
                       │ MBR partition scheme         │
                       │ Intel Fast Flash             │
                       │ BIOS boot                    │
                       │ Sony boot partition          │
                       │ Lenovo boot partition        │
                       │ PowerPC PReP boot            │
                       │ ONIE boot                    │
                       │ ONIE config                  │
                       │ Microsoft reserved           │
                       │ Microsoft basic data         │
                       │ Microsoft LDM metadata       │
                       │ Microsoft LDM data           │
                       │ Windows recovery environment │
                       │ IBM General Parallel Fs      │
                       │ Microsoft Storage Spaces     │
                       │ HP-UX data                   │
                       │ HP-UX service                │
                       │ Linux swap                   │
                       └────────────────────────────↓─┘

                     C12A7328-F81F-11D2-BA4B-00A0C93EC93B
```

tekan tombol `enter` untuk memilih, selanjutnya kita akan kembali ke menu utama, pada menu bagian bawah pilih menu `[ Write ]`

```shell
                                Disk: /dev/sda
              Size: 7.26 GiB, 7798784000 bytes, 15232000 sectors
         Label: gpt, identifier: B81DEE59-E012-4A1F-ADED-E8B1C110177D

    Device               Start          End      Sectors     Size Type
>>  /dev/sda1             2048      3909631      3907584     1.9G EFI System   
    /dev/sda2          3909632     15230975     11321344     5.4G unknown






 ┌───────────────────────────────────────────────────────────────────────────┐
 │ Partition UUID: BD619216-97B6-4245-9E0B-0DD7E121F602                      │
 │ Partition type: EFI System (C12A7328-F81F-11D2-BA4B-00A0C93EC93B)         │
 │Filesystem UUID: 38CD-98E8                                                 │
 │     Filesystem: vfat                                                      │
 └───────────────────────────────────────────────────────────────────────────┘
    [ Delete ]  [ Resize ]  [  Quit  ]  [  Type  ]  [  Help  ]  [  Write ]
    [  Dump  ]
                         Changed type of partition 1.
```

selanjutnya pilih menu `[ Quit ]`. Sampai pada tahap ini kita tinggal melakukan format terhadap paritisi `/dev/sda1` dengan perintah.

```shell
 mkfs.fat /dev/sda1
```

ganti `/dev/sda1` dengan path yang anda gunakan masing masing.

## Menambahkan luks key kedalam usb

mount paritisi `/dev/sda1` ke direktori `mnt` dengan perintah:

```shell
mount /dev/sda1 /mnt
```

selanjutnya generate random key

```shell
dd if=/dev/urandom of=/mnt/karim.key bs=4096 count=1
```

ganti `davinci.key` dengan nama yang anda inginkan. output dari perintah tersebut adalah:

```shell
dd if=/dev/urandom of=/mnt/davinci.key bs=4096 count=1
1+0 records in
1+0 records out
4096 bytes (4.1 kB, 4.0 KiB) copied, 0.000481918 s, 8.5 MB/s
```

selanjutnya kita tambahkan key kedalam volume luks. sebelumnya kita cek terlebih dahulu dengan perintah :

```shell
lsblk
```

output dari perintah tersebut

```shell
NAME                            MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda                               8:0    1   7.3G  0 disk  
├─sda1                            8:1    1   1.9G  0 part  /mnt
└─sda2                            8:2    1   5.4G  0 part  
  └─luks-abc8e9f3-1186-4ef3-b2f0-3ed275e07365
                                253:5    0   5.4G  0 crypt /run/media/null/datums
nvme0n1                         259:0    0 476.9G  0 disk  
├─nvme0n1p1                     259:1    0   260M  0 part  /boot/efi
├─nvme0n1p2                     259:2    0    16M  0 part  
├─nvme0n1p3                     259:3    0 250.9G  0 part  
├─nvme0n1p4                     259:4    0   1.1G  0 part  
├─nvme0n1p5                     259:5    0     1G  0 part  /boot
└─nvme0n1p6                     259:6    0 223.6G  0 part  
  └─luks-36ea0348-6860-4926-b799-9dca2f7244eb
                                253:0    0 223.6G  0 crypt 
    ├─rhel_davinci-root         253:1    0    30G  0 lvm   /
    ├─rhel_davinci-swap         253:2    0   7.7G  0 lvm   [SWAP]
    ├─rhel_davinci-home         253:3    0 115.9G  0 lvm   /home
    └─rhel_davinci-var_lib_libvirt
                                253:4    0    70G  0 lvm   /var/lib/libvirt
```

dari informasi di atas kita mengetahui path paritisi yang terenkripsi dengan luks adalah partisi dengan nama `nvme0n1p6`. sekarang kita menambahkan key ke partisi `nvme0n1p6` dengan perintah:

```shell
cryptsetup luksAddKey /dev/nvme0n1p6 /mnt/davinci.key
```

`/mnt/davinci.key` adalah path dari paritisi usb yang kita mount sebelumnya dan `/dev/nvme0n1p6` adalah paritisi yang di enkripsi menggunakan luks. anda akan diminta memasukan password luks yang telah tersimpan secara manual.

```shell
cryptsetup luksAddKey /dev/nvme0n1p6 /mnt/davinci.key
Enter any existing passphrase: 
```

selanjutnya kita akan mencoba membuka paritisi luks menggunakan kunci yang telah kita buat.

```shell
cryptsetup luksOpen --key-file /mnt/davinci.key --test-passphrase /dev/nvme0n1p6 && echo "Key register." || echo "Key not found."
```

Jika output dari comment tersebut adalah `key register` maka key telah berhasil ditanamkan ke partisi luks,

## Rekonfigurasi Crypttab

pertama kita haru mengetahui terlebih dahulu UUID usb drive, untuk mengetahui UUID dari usb yang kita gunakan bisa menggunakan perintah:

```shell
lsblk /dev/sda1 -o UUID
```

 output dari perintah tersebut adalah:

```shell
UUID
38CD-98E8
```

Tambahkan syntax berikut pada file `/etc/crypttab` dengan perintah:

```shell
vi /etc/crypttab
```

outputnya adalah

```shell
luks-36ea0348-6860-4926-b799-9dca2f7244eb UUID=36ea0348-6860-4926-b799-9dca2f7244eb none discard
```

ganti `none` dengan format

```shell
keyfile:UUID=USB_UUID 
```

dari tutorial ini maka valuenya adalah

```shell
davinci.key:UUID=38CD-98E8 
```

dan tambahkan ganti value `discard` dengan

```shell
 discard,keyfile-timeout=5s
```

dari proses diatas maka yang pada awalnya output dari perintah `vi /etc/crypttab` adalah :

```shell
luks-36ea0348-6860-4926-b799-9dca2f7244eb UUID=36ea0348-6860-4926-b799-9dca2f7244eb none discard
```

berubah menjadi 

```shell
luks-36ea0348-6860-4926-b799-9dca2f7244eb UUID=36ea0348-6860-4926-b799-9dca2f7244eb davinci.key:UUID=38CD-98E8 discard,keyfile-timeout=5s
```

## Rekonfigurasi GRUB2

edit default konfigurasi grub dengan perintah

```shell
vi /etc/default/grub
```

berikut format syntax yang akan di tambahkan

```shell
rd.luks.key=LUKS_DRIVE_UUID=keyfile:UUID=USB_DRIVE_UUID rd.luks.options=timeout=5s
```

berdasarkan keterangan sebelumnya maka kita akan menggunakan sintax berikut, jangan lupa disesuikan dengan informasi perangkat anda karena setiap perangkat akan berbeda beda.

```shell
rd.luks.key=36ea0348-6860-4926-b799-9dca2f7244eb=davinci.key:UUID=38CD-98E8 rd.luks.options=timeout=5s
```

tambahkan systax diatas setelah property    `rd.luks.uuid`, berikut konfigurasi default dari file `/etc/default/grub`

```shell
GRUB_TIMEOUT=0
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="resume=/dev/mapper/rhel_davinci-swap rd.luks.uuid=luks-36ea0348-6860-4926-b799-9dca2f7244eb rd.lvm.lv=rhel_davinci/root rd.lvm.lv=rhel_davinci/swap rhgb quiet splash fips=1 boot=UUID=47ab35c0-4102-41ee-a131-a5efa446e984"
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG=true
GRUB_DISABLE_OS_PROBER=true
```

setelah ditambahkan maka berubah menjadi 

```shell
GRUB_TIMEOUT=0
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="resume=/dev/mapper/rhel_davinci-swap rd.luks.uuid=luks-36ea0348-6860-4926-b799-9dca2f7244eb rd.luks.key=36ea0348-6860-4926-b799-9dca2f7244eb=davinci.key:UUID=38CD-98E8 rd.luks.options=timeout=5s rd.lvm.lv=rhel_davinci/root rd.lvm.lv=rhel_davinci/swap rhgb quiet splash fips=1 boot=UUID=47ab35c0-4102-41ee-a131-a5efa446e984" 
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG=true
GRUB_DISABLE_OS_PROBER=true
```

update initramfs 

```shell
dracut -f
```

selanjutnya generate GRUB2 config dengan format, sesuaikan dengan perangkat anda masing masing.

```shell
grub2-mkconfig -o /boot/efi/EFI/[distro_name]/grub.cfg
```

contoh dari command di atas adalah:

```shell
grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
```

lalu restart perangkat anda untuk uji coba.
