---
layout: post
title: C Programming Language
categories : Programming
tags : [C, Language, Programming]
---
{% include JB/setup %}

## Control Flow

## Standard Library
* Input and Output `stdio.h`
* Character Class Tests `<ctype.h>`
* String Functions `<string.h>`
* Utility Functions `<stdlib.h>`
* Mathematical Functions `<math.h>`
* Diagnostics `<assert.h>`
* Date and Time Functions `<time.h>`

### `<ctype.h>`
用于判断是什么字符，或用于字符转换
* 参数：每个函数的参数都是一个int，值必须为EOF或一个unsigned char
* 返回值：一个int，返回非零表示参数满足条件描述，零表示不满足条件描述

包含两类
* is ... Functions
* to ... Functions 

```
int tolower(int character);
int toupper(int character);
```
![字符方法图]({{ BASE_PATH }}/assets/pics/ctype_character.png)

## 相关书籍
* [The C Programming Language]
