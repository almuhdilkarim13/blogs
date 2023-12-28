---
draft     : false
author    : "Al Muhdil Karim"
title     : "Install dnsmasq pada Redhat Plow 9.3 KVM"
slug      : "install-dnsmasq-pada-rhel-kvm"
images    : ""
date      : 2023-12-28
lastmod   : ""
categories: [ DevOps ]
tags      : [ rhelp, dnsmasq ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Cara melakukan konfigurasi ip4 pada terminal di linux menggunakan perintah ip"
---

## Installasi

install dns mask dengan menggunakan perintah berikut

```shell
sudo dnf install dnsmasq
```

## Konfigurasi

copy default config dnsmasq dengan perintah berikut

```shell
sudo cp /etc/dnsmasq.conf /etc/dnsmasq.conf.origin
```

kosong file konfigurasi dengan perintah

```shell
echo '' > /etc/dnsmasq.conf
```

selanjutnya isikan konfigurasi berikut

```shell
##  prevent packets private ip leaving local network
domain-needed
bogus-priv

## limits your name services exclusively to dnsmasq
no-resolv

## interface configuration
interface=eth0
bind-interfaces
listen-address=127.0.0.1, 172.275.50.1
cache-size=1000
no-poll

## local server
domain=fahmaya.com
server=/fahmaya.com/127.0.0.1
server=/fahmaya.com/172.275.50.1

## upstream dns
server=1.1.1.1
server=8.8.8.8


## Can append below two parameters to log host queries
# log-queries
# log-facility=/var/log/dnsmasq.log
```

Lakukan pengencekan konfigurasi dengan perintah

```shell
sudo dnsmasq --no-daemon --log-queries
```

aktivkan service dan auto enable saat proses booting

```
systemctl enable --now dnsmasq
```

## konfigurasi firewall

```shell
sudo firewall-cmd --permanent --zone=public --add-service=dns
```

```shell
sudo firewall-cmd --permanent --zone=public --add-service=dhcp
```

jika ada zone lainya pada firewalld silahkan tambahkan. setelah itu reload konfigurasi firewalld dengan perintah

```shell
firewall-cmd --reload
```

## Pengujian

```shell
dig fahmaya.comn
```

## Troubleshoot

### failed start service on boot

cek status dengan perintah

```shell
sudo systemctl status dnsmasq
```

contoh status service seperti contoh berikut

```shell
× dnsmasq.service - DNS caching server.
     Loaded: loaded (/usr/lib/systemd/system/dnsmasq.service; enabled; preset: disabled)
     Active: failed (Result: exit-code) since Wed 2023-12-27 18:10:51 EST; 55s ago
    Process: 884 ExecStart=/usr/sbin/dnsmasq (code=exited, status=2)
        CPU: 33ms

Dec 27 18:10:51 turing systemd[1]: Starting DNS caching server....
Dec 27 18:10:51 turing dnsmasq[884]: dnsmasq: failed to create listening socket for 172.27.5.101: Cannot assign requested address
Dec 27 18:10:51 turing dnsmasq[884]: failed to create listening socket for 172.27.5.101: Cannot assign requested address
Dec 27 18:10:51 turing dnsmasq[884]: FAILED to start up
Dec 27 18:10:51 turing systemd[1]: dnsmasq.service: Control process exited, code=exited, status=2/INVALIDARGUMENT
Dec 27 18:10:51 turing systemd[1]: dnsmasq.service: Failed with result 'exit-code'.
Dec 27 18:10:51 turing systemd[1]: Failed to start DNS caching server..
```

buka file `/usr/lib/systemd/system/dnsmasq.service` dengan perintah

```shell
vi /usr/lib/systemd/system/dnsmasq.service
```

jike konfigurasi file seperti di bawah ini

```shell
[Unit]
Description=DNS caching server.
After=network.target

[Service]
ExecStart=/usr/sbin/dnsmasq
Type=forking
PIDFile=/run/dnsmasq.pid

[Install]
WantedBy=multi-user.target
~                         
```

maka ganti menjadi

```shell
[Unit]
Description=DNS caching server.
Wants=network-online.target
After=network.target network-online.target

[Service]
ExecStart=/usr/sbin/dnsmasq
Type=forking
PIDFile=/run/dnsmasq.pid

[Install]
WantedBy=multi-user.target
~                         
```

selanjutnya restart virtual guest untuk menguji konfigurasi tersebut.

### Reference

https://oss.segetech.com/intra/srv/dnsmasq.conf

[How to configure DNS caching server with dnsmasq in RHEL - Red Hat Customer Portal](https://access.redhat.com/solutions/2189401)

[Installing DNS Server on CentOS/RHEL using dnsmasq | Zimbra - Zextras Community](https://community.zextras.com/dns-server-installation-guide-on-centos-7-rhel-7-and-centos-8-rhel-8-using-dnsmasq/)

[Advanced Dnsmasq Tips and Tricks - Linux.com](https://www.linux.com/topic/networking/advanced-dnsmasq-tips-and-tricks/)

[systemd - Cause a script to execute after networking has started? - Unix &amp; Linux Stack Exchange](https://unix.stackexchange.com/questions/126009/cause-a-script-to-execute-after-networking-has-started)

[networking - dnsmasq not starting on boot - Raspberry Pi Stack Exchange](https://raspberrypi.stackexchange.com/questions/106531/dnsmasq-not-starting-on-boot)


