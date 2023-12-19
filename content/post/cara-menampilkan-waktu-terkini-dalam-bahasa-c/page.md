---
draft     : false
author    : Al Muhdil Karim
title     : "Cara mendapatkan dan menampilkan waktu pada pemograman C"
date      : 2021-04-15
lastmod   : ""
apps       : ""
programing : "c"
related1 : "yum"
related2 : "fedora"
related3 : "Package Manager"
related4 : "DevOps"
categories: [ Programing ]
tags      : [ C ]
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



```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
 
// Print the current date and time in C
int main(void)
{
    // variables to store the date and time components
    int hours, minutes, seconds, day, month, year;
 
    // `time_t` is an arithmetic time type
    time_t now;
 
    // Obtain current time
    // `time()` returns the current time of the system as a `time_t` value
    time(&now);
 
    // Convert to local time format and print to stdout
    printf("Today is %s", ctime(&now));
 
    // localtime converts a `time_t` value to calendar time and
    // returns a pointer to a `tm` structure with its members
    // filled with the corresponding values
    struct tm *local = localtime(&now);
 
    hours = local->tm_hour;         // get hours since midnight (0-23)
    minutes = local->tm_min;        // get minutes passed after the hour (0-59)
    seconds = local->tm_sec;        // get seconds passed after a minute (0-59)
 
    day = local->tm_mday;            // get day of month (1 to 31)
    month = local->tm_mon + 1;      // get month of year (0 to 11)
    year = local->tm_year + 1900;   // get year since 1900
 
    // print local time
    if (hours < 12) {    // before midday
        printf("Time is %02d:%02d:%02d am\n", hours, minutes, seconds);
    }
    else {    // after midday
        printf("Time is %02d:%02d:%02d pm\n", hours - 12, minutes, seconds);
    }
 
    // print the current date
    printf("Date is: %02d/%02d/%d\n", day, month, year);
 
    return 0;
}

```



## Ouput

Today is Fri Jul 30 11:44:17 2021
Time is 11:44:17 am
Date is : 30/07/2021
