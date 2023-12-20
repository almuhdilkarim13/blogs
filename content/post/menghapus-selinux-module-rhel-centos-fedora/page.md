---
draft     : false
author    : "Al Muhdil Karim"
title     : "Menghapus custom policy atau module pada RHEL, Centos dan Fedora "
slug      : "menghapus-custom-selinux-policy-module-rhel-centos-fedora"
images    : ""
date      : 2023-12-19
lastmod   : ""
categories: [ DevOps ]
tags      : [ security, selinux ]
math      : false
seo   :
    index       : ""
    nofollow    : ""
    keyword     : ""
    cover       : ""
    description :  "Untuk menghapus module yang didaftarkan secara manual pada selinux maka kita bisa melakukan dengan cara berikut"
---





Untuk menghapus module kita bisa memulai dari melakukan pengecekan terhadap module selinux yang telah di implementasikan menggunakan perintah berikut:



```shell
sudo semodule --list=full
```



contoh dari output dari perintah tersebut adalah sebagai berikut



```shell
200 pasta             pp          
200 swtpm             pp          
200 swtpm_svirt       pp          
200 usbguard          pp          
100 abrt              pp          
100 accountsd         pp          
100 acct              pp          
100 afs               pp         
--- dan seterusnya --- 
```



angka `200` adalah nilai prioritas, kemudian di ikuti dengan nama module. dalam contoh kali ini kita akan menghapus module `pasta` dengan nilai priority `200` . 



Silahkan sesuaikan nama module dan nilai priority dengan module yang ingin anda hapus. Untuk menghapus module selinux bisa menggunakan perintah di bawah ini.



```shell
semodule -X<priority> -r <modulename>
```

sebagai contoh, jika ingin menghapus module `pasta` berdasarkan list di atas maka kita bisa mengugunakan perintah



```shell
semodule -X 200 -r pasta
```

selanjutnya lakukan pengecekan kembali dengan perintah sebagai berikut



```shell
sudo semodule --list=full | grep "pasta"
```



Jika tidak ada hasil yang ditampilkan berarti module telah berhasil di hapus dari selinux.




