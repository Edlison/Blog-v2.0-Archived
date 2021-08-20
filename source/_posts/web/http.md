---
title: HTTP comprehension
categories:
- Web
tags:
- http
---

# Introduction

## Features

- no connection: process one request in each connection.
- no status: if current transaction needs previous information, it should be transfered again.
- media independent: any type of media that is able to process can be transferred.

## URI & URL

URI (Uniform Resource Identifiers), is a abstract concept.

URL (Uniform Resource Locator), a specific URI, can locate a specific resource.

# Request

## Request contains

- Request line(Method, URL, HTTP version)
- Header(key&value)
- CR LF
- Body(Only in POST method*)

## GET

```http
GET /xxxxxxx.jpg HTTP/1.1  # section 1
Host:img.mukewang.com  # section 2
Accept:image/webp,image/*,*/*;q=0.8
Accept-Encoding:gzip, deflate, sdch
Accept-Language:zh-CN,zh;q=0.8
CR(carriage reture) LF(line feed)  # section 3
```

Section1: Request line

Section2: Header

Section3: CR LF

Body not in GET method.

## POST

```http
POST / HTTP1.1  # seciton 1
Host:www.wrox.com  # section 2
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive
  # section 3
name=Professional%20Ajax&publisher=Wiley  # section 4
```

Section1: Request line

Section2: Header

Section3: CR LF

Section4: Body(type defined by Content-Type in Header)

## GET & POST

Usually GET method pass value through params which is attached to URL, and body is empty. Although in current HTTP version, GET method pass value through body is accepted. We should follow the common sense that do not put value in body.

# Respond

## Respond contains

- Respond line(HTTP version, Status code, Reason phrase)
- Header
- CR LF
- Body

```http
HTTP/1.1 200 OK  # section 1
Date: Fri, 22 May 2009 06:07:21 GMT  # section 2
Content-Type: text/html; charset=UTF-8
  # section 3
<html>  # section 4
      <head></head>
      <body>
            <!--body goes here-->
      </body>
</html>
```

Section1: Respond line

Section2: Header

Section3: CR LF

Section4: Body

# Parameters

## Status code

| code | describe     |
| ---- | ------------ |
| 1xx  | infomation   |
| 2xx  | success      |
| 3xx  | redirection  |
| 4xx  | client error |
| 5xx  | server error |

## Content-Type

**application/x-www-form-urlencoded**

default method, only tansfer text

```
POST  HTTP/1.1
Host: test.app.com
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache
Postman-Token: e00dbaf5-15e8-3667-6fc5-48ee3cc89758

key1=value1&key2=value2
```

**multipart/form-data**

transfer text or files

```
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="filekey"; filename=""
Content-Type: 


------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="textkey"
```

You can transfer text and files at one time. The Body is seperated by boundary.

**application/json**

assign date type in Body

```
{
	'name': 'edlison'
}
```

You need to serialize the data before tansfering.







----

Reference

https://www.cnblogs.com/ranyonsue/p/5984001.html

https://zhuanlan.zhihu.com/p/45173862

https://www.cnblogs.com/wbl001/p/12050751.html