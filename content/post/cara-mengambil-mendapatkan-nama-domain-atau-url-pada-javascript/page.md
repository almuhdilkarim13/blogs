---
draft      : false
author     : Al Muhdil Karim
title      : "Fungsi Javascript Terkait Nama Domain, Url, Hostname dan Port "
date       : 2021-08-07
lastmod    : ""
apps       : ""
programing : "javascript"
related1  : "javascript"
related2  : "snipets"
related3  : "Frontend"
related4  : "Programing"
categories : [ Programing ]
tags       : [ Javascript ]
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
    description : "Dalam membuat suatu aplikasi, terkadang kita membutuhkan informasi terkait domain, hostname, url , port dan protokol. Berikut ini adalah fungsi javascript untuk mengetahui informasi di atas"
---
## Megetahui hash url
```js
window.location.hash
```
output : ```#2```

## Mengetahui host 
```js
window.location.hostname
```
output : ```localhost```

## Mengambil host dan port
```js
window.location.host
```
output : ```localhost:4200```

## Mengambil full url
```js
window.location.href
```
output : ```http://localhost:4200/landing?query=1#2```

## Mengambil origin url
```js
window.location.origin
```
output : ```http://localhost:4200```

## Mengambil port yang digunakan
```js
window.location.port
```
output : ```4200```

## Mengambil protocol yang digunakan
```js
window.location.protocol
```
output : ```http:​```

## Mengambil lokasi pencarian 
```js
window.location.search
```
output : ```?query=1​```

## Mengambil path name
```js
window.location.pathname
```
output : ```/landing```