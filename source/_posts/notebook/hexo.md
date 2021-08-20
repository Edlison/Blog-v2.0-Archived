---
title: Config in Next theme
categories:
- Notebook
tags:
- hexo
---

# Pagination

Edit `_config.yml` in blog.

```
index_generator:
  path: ''
  per_page: 5
  order_by: -date
```

# SEO

Edit `_config.yml` in `next` theme.

```
# Google Analytics
google_analytics:
  tracking_id: G-1B9EHJMCZY
  # By default, NexT will load an external gtag.js script on your site.
  # If you only need the pageview feature, set the following option to true to get a better performance.
  only_pageview: false
```

Due to the anti-spider, SEO on the website that deployed on github doesn't work.

## Plan A

`Vercel.com`



## Plan B

Establish another git repo on `coding.net`



# Excerpt

## method1

```
---
title: xxx
description: xxxxxxxxx
---
```

## method2

Add this code to your content where you want to truncate.

```
<!--more-->
```



----

Reference

https://blog.csdn.net/yueyue200830/article/details/104470646