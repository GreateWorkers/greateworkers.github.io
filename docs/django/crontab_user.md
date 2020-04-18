---
layout: default
title: Django logging & crontab logging 
nav_order: 6
parent: django
previous: webpack
next: ../django
---
{% include nav_btn.html %}

# Django logging & crontab logging
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 장고의 logging 과 crontab logging

사용자 deploy 로 로그인해서 crontab 을 등록해야  
권한 문제가 발생하지 않음

권한을 추가하여도 logging 파일이 백업되면  
권한 에러가 나니

```
sudo setfacl -R -m u:ubuntu:rwx log
```

deploy 계정으로 crontab 을 등록

```
crontab -u deploy -e
crontab -u deploy -l
```



{% include nav_btn.html %}
