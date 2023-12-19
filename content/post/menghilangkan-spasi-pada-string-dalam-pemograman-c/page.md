---
draft     : false
author    : Al Muhdil Karim
title     : "Menghilangkan Spasi Pada String Dalam Pemograman C"
date      : 2021-07-30
lastmod   : ""
apps       : ""
programing : "c"
related1   : "C"
related2   : "Guide"
related3   : "Backend"
related4   : "Programing"
categories : [ Programing ]
tags       : [ C ]
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
    description : "Terkadang perangkat tidak mendukung penggunaan IPV6, Nah bagaimana kita bisa mengatur package manager YUM hanya menggunakan IPV4 pada Fedora dan Centos"
---

Program try to neglecting the White spaces and blanks from given string to display only valid characters

```
#include <stdio.h>
 
int main()
{
   char text[100], blank[100];
   int c = 0, d = 0;
 
   printf("Enter some text\n");
   gets(text);
 
   while (text[c] != '\0')
   {
      if (!(text[c] == ' ' && text[c+1] == ' ')) {
        blank[d] = text[c];
        d++;
      }
      c++;
   }
 
   blank[d] = '\0';
 
   printf("Text after removing blanks\n%s\n", blank);
 
   return 0;
}
 ```

Contoh 2
```
char* createpostslug( char slug[150] )
{
  char title[150];
  int c = 0, d = 0;

  printf("Enter post title : ");
  fgets(title, sizeof title, stdin);
  
  while (title[c] != '\0')
  {
    if (!(title[c] == ' ')) {
      slug[d] = tolower(title[c]);
      d++;
    } else {
      slug[d] = '-';
      d++;
    }
    c++;
  }

  title[d] = '\0';
  return slug ;
}
```