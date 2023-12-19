---
draft     : false
author    : "Al Muhdil Karim"
title     : "Setting Ip Statis Menggunakan Perintah Ip Pada Linux"
slug      : "setting-ip-statis-menggunakan-perintah-ip-pada-linux"
images    : ""
date      : 2023-12-20
lastmod   : ""
categories: [ DevOps ]
tags      : [ linux, ipv4 ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Menginstall lynis dengan mudah dengan berbaagai metode dan untuk berbagai distro linux "

---

## Pilih perangkat

untuk mengetahui nama perangkat ketikan perintah berikut di terminal

```shell
ip address
```

   contoh output dari perintah tersebut adalah

```shell
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel master bridge state UP group default qlen 1000
    link/ether 98:ee:cb:f1:ff:9c brd ff:ff:ff:ff:ff:ff
3: wlp0s20f3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether ba:90:ab:43:d8:eb brd ff:ff:ff:ff:ff:ff permaddr f8:b5:4d:44:d2:52
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:3b:db:1d brd ff:ff:ff:ff:ff:ff
    inet 192.168.100.1/24 brd 192.168.100.255 scope global virbr0
       valid_lft forever preferred_lft forever
```

untuk tutorial ini kita akan menggunakan perangkat ethernet dengan nama `enp2s0`

## Keterangan

dalam kesempatan ini kita akan melakukan konfigurasi ip kepada perangkat `enp2s0` sebagai berikut:

| Keterangan | Nilai         |
| ---------- | ------------- |
| ip address | 172.27.5.82   |
| netmask    | 255.255.255.0 |
| gateway    | 172.27.5.1    |
| dns        | 1.1.1.1       |



## Konfigurasi IP

langkah pertama adalah melakukan konfigurasi ip perangkat yang disertai dengan prefix cidr nya;

```shell
ip address add 172.27.5.82/24 broadcast + dev enp2s0
```

broadcast adalah alamat broadcast perangkat untuk nilai default silahkan masukan tanda `+` sedangkan `dev` pada perintah diatas adalah nama perangkat yang akan disetting.



## Konfigurasi DNS



selanjutnya kita akan melakukan konfigurasi dns periksa file   `/etc/resolve.conf` dengan perintah 



```shell
cat /etc/resolv.conf
```

temukan line berikut dari output perintah di atas

```shell
nameserver 1.1.1.1
```

jika ditemukan lanjutkan pada proses konfigurasi Gateway, namun jika tidak ditemukan maka ketikan perintah berikut pada terminal

```shell
echo "nameserver 1.1.1.1" > /etc/resolv.conf
```



## Konfigurasi Gateway

Untuk konfigurasi gateway yang pertama adalah menambahkan route pada perangkat `enp2s0` dengan ip gateway router menggunakan perintah berikut.

```shell
ip route add 172.27.5.1 dev enp2s0
```

Jadikan ip gateway sebagai default gateway bagi perangkat `enp2s0` dengan perintah

```shell
ip route add default via 172.25.5.1 dev enp2s0
```

sampai disini kita telah berhasil menambahkan ip statis pada perangkat `enp2s0`.  



## Test Koneksi

untuk melakukan test koneksi terhadap konfigurasi di atas, kita bisa menggunkan perintah `ping` , berikut contohnya:

```shell
ping 8.8.8.8
```

jika output dari perintah diatas seperti contoh dibawah ini maka konfigurasi telah berhasil di lakukan



```shell
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=16.5 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=16.6 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=118 time=16.6 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=118 time=16.6 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=118 time=16.6 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=118 time=16.5 ms
^C
--- 8.8.8.8 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5005ms
rtt min/avg/max/mdev = 16.492/16.572/16.642/0.055 ms

```


