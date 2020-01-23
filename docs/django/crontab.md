---
layout: default
title: Django Crontab 에 scrapy 등록
nav_order: 4
parent: django
previous: 3
next: webpack
---
{% include nav_btn.html %}

# Django Crontab 에 scrapy 등록
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## django-crontab

pip install django-crontab

[문서 참조](https://pypi.org/project/django-crontab/)

install via pip:
```
pip install django-crontab  
```

settings.py:

```
INSTALLED_APPS = (
    'django_crontab',
    ...
)
```

---
### 웹서버에서 스케줄링  
myapp/cron.py:

```
def my_scheduled_job():
  pass
```

등록하기
settings.py:  
```
CRONJOBS = [
    ('*/5 * * * *', 'myapp.cron.my_scheduled_job')
]
```

### management command 을 등록하기

```
CRONJOBS = [
    ('*/5 * * * *', 'myapp.cron.other_scheduled_job', ['arg1', 'arg2'], {'verbose': 0}),
    ('0   4 * * *', 'django.core.management.call_command', ['clearsessions']),
]
```

command 가 argument 가 있을 경우는 
```
'django.core.management.call_command', ['clearsessions'] , {'name':'arg'}),
```

log 를 찍고 싶으면

```
'django.core.management.call_command', ['clearsessions'] , {'name':'arg'},'>> /logpath/a.log 2>&1'),
```

---
### crontab 에 등록
등록 (수정해서 다시 등록하거나 하면 오류나는데 한번 더 실행하면 등록됨)

```
python manage.py crontab add
```

보기

```
python manage.py crontab show
```

지우기

```
python manage.py crontab remove
```

config ...(문서참조)


---
## scrapy

- scrapy 을 맥이나 로컬에서 django-crontab 에 등록가능  
- ubuntu 에서는 안됨
- scrapy 는 global/project 명령어가 나눠져 있음
- ubuntu 실행 시  
project not found ? crawl 이런 오류가 남  
[Configuration settings](https://docs.scrapy.org/en/latest/topics/commands.html#configuration-settings) 뭘 하면 될 것 같은데...
- 어쨋든 되지 않아 직접 등록

---
### django-crontab 에 scrapy 맥/로컬 등록
개발 단계에서 맥이나 로컬에 scrapy을 call command 에 등록하는 방법

```
from scrapy.cmdline import execute
execute(['scrapy', 'crawl', 'quotes'])
```
---
### scrapy ubuntu crontab 에 등록

프로젝트 폴더로 이동하여 (scrapy.cfg 가 있는 곳)
management command 를 실행한다

```
40 6 * * * cd /home/ubuntu/project && /home/ubuntu/virtualenv/bin/python /home/ubuntu/project/manage.py (management command name) 
```

혹은

```
40 6 * * * cd /home/ubuntu/project && /home/ubuntu/virtualenv/bin/scrapy crawl quotes
```

## 한글이 깨질 경우

scrapy 실행 시 crawl 된 데이타가 깨질 경우  
맥 로컬 등에서 잘 되는 데 깨진다면  

ubuntu LANG을 확인
```
echo $LANG
```
LANG=ko_KR.UTF-8
이 아닌 en_으로 되어 있을 경우

```
sudo vi /etc/default/locale  
LANG=ko_KR.UTF-8  

sudo reboot 
```

우분투 부팅 메세지가 나온다 
시키는 데로 설치하면 잘 나온다

아래와 같은 문구였던 것 같다 어렵지 않다  
```
sudo apt-get install language-pack-ko 
sudo locale-gen ko_KR.UTF-8 
```

## LANG=ko_KR.UTF-8

참고) crontab 라인에 추가해줘도 안됨
40 6 * * * LANG=ko_KR.UTF-8 [명령어] 
위에 방법으로 해야 됨

{% include nav_btn.html %}
