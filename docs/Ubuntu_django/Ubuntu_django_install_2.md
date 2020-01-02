---
layout: default
title: ubuntu 에 django 설치
nav_order: 2
parent: Ubuntu django 설치
---

# Virtual Machine 에 ubuntu 설치
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
 
## 설치 환경
2.4 intel core 2 duo  
macbook pro 13inch 2010   
osx 10.14 mojave   
16g ram  

---

## package 설치

<div class="code-example" markdown="1">
파이선 3.6을 설치합니다.
</div>

```
sudo apt-get install python3.6 python3-dev python3-pip  
```

<div class="code-example" markdown="1">
nginx 를 설치합니다.   
</div>

```
sudo apt-get install nginx  
```

<div class="code-example" markdown="1">
pip을 업그레이드합니다.  
</div>

```
sudo -H pip3 install --upgrade pip  
```


<div class="code-example" markdown="1">
가상환경을 설치합니다.  
</div>

```
sudo -H pip3 install wheel    
sudo -H pip3 install virtualenv   

```



<div class="code-example" markdown="1">
uwsgi는 pip을 통해 가상환경에 설치.  
uwsgi를 관리하는 방식에 따라 아래와 같이 할 수 있음  
난 Django 가상환경에 pip으로 설치하여 사용  

</div>

```
sudo -H pip3 install uwsgi  
```

---

## virtualenv 설정

- 파이선의 버전을 지정하여 가상환경을 설치.  
차후 pip 패키지 설치 시 파이선 버전오류가 발생하지 않음.    

<div class="code-example" markdown="1">
가상환경 설치    
</div>
```
virtualenv --python=python3.6 myvenv  
```

<div class="code-example" markdown="1">
가상환경 실행  
</div>

```
. myvenv/bin/activate  
```

---

## django 설치

<div class="code-example" markdown="1">
django 설치
</div>

```
pip install django  
```

<div class="code-example" markdown="1">
장고에 webserver 프로젝트 생성
</div>

```
django-admin startproject webserver    
```

<div class="code-example" markdown="1">

- 슈퍼 유저 생성 (관리자 페이지 접속 가능)  
이름 / 이메일 등을 입력 ( 이름만 입력하고 패스가능 )

- 마이그레이션 / 마이그레이트 실행
데이타베이스 생성 및 적용

- 로컬 서버 실행
서버가 실행되면서 웹브라우저에서 접속할 주소가 표시된다
직접할려면
</div>

```
python manage.py createsuperuser
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
python manage.py runserver 127.0.0.1:8000
python manage.py runserver 127.0.0.1:8000 --settings=[사용할 셋팅 파일지정]
```

![image](/assets/images/n/4.jpeg)  

![image](/assets/images/n/7.jpeg)  

![image](/assets/images/n/8.jpeg)  

---

## uwsgi 설정 파일


<div class="code-example" markdown="1">

- 빠른 웹서비스와 보안을 위해 uwsgi 와 nginx 를 셋팅  
- uwsgi ini 파일 생성하여 실행한다  
- uid / gid 의 deploy 는 www-data 로 변경 가능  
nginx 의 기본 유저는 www-data 임

</div>

---

<div class="code-example" markdown="1">

- ubuntu 에서 실제 서비스할 설정 파일 deploy.ini

</div>

```
[uwsgi]
sitename        = mysite 
chmod-socket    = 660 
uid             = deploy 
gid             = deploy
home            = /home/사용자이름/%(sitename)/myvenv
chdir           = /home/사용자이름/%(sitename)/webserver
socket          = /home/사용자이름/%(sitename)/tmp/%(sitename).sock
pidfile         = /home/사용자이름/%(sitename)/tmp/%(sitename).pid
logto           = /home/사용자이름/%(sitename)/tmp/@(exec://date +%%Y-%%m-%%d).log
chown-socket    = %(uid):%(gid)
module          = server.wsgi.remote:application
thunder-lock    = true
enable-threads  = true
vacuum          = true
log-reopen      = true

```
<div class="code-example" markdown="1">

- 맥에서 디버그용 debug.ini

</div>

```
[uwsgi]
home            = mypath/myvenv
chdir           = mypath/webserver
socket          = mypath/webserver/uwsgi/tmp/mysock.sock
pidfile         = mypath/webserver/uwsgi/tmp/mypid.pid
module          = server.wsgi.debug
chmod-socket=666
```

<div class="code-example" markdown="1">

- 로컬 저장 위치는 mypath/webserver/uwsgi 에 저장

- 실제 서비스할 곳은 uwsgi emperor 를 위해 한 곳에 모아둔다

</div>

![image](/assets/images/n/1.jpeg)  

---

## nginx 설정 파일

```
upstream my_sock {
    server unix:///[mypath]/mysock.sock;
}

server {
    //192.168.x.x:8000 or 192.168.x.x 가능
    server_name [mysite.com]; 
    charset utf-8;

    location /media  {
        alias /[mypath]/media;
    }
    location /static  {
        alias /[mypath]/static;
    }    
    location / {
        uwsgi_pass my_sock;
        include /etc/nginx/uwsgi_params;        
    }
}

```

## 권한 설정

<div class="code-example" markdown="1">

폴더 권한 변경

- tmp 폴더의 권한을 deploy,www-data 등의 서비스용 계정으로 변경해준다
- 파일 생성이 필요하다면 media 도 
- 루트에 있는 run, tmp 등의 폴더 사용은 권장하지 않는 것 같다
</div>

```
sudo chown -R [서비스용 계정]:[서비스용 계정 그룹] tmp
sudo chown -R [서비스용 계정]:[서비스용 계정 그룹] media

예) sudo chown -R deploy:deploy tmp
```

---


[이전 단계](../Ubuntu_VirtualMachine_1) |
[다음 단계](../Ubuntu_django_error_3)