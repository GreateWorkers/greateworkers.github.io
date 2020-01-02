---
layout: default
title: nginx uwsgi
nav_order: 4
parent: Lightsail 웹서비스 시작하기
---

# nginx uwsgi
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## uwsgi 설정 파일

<div class="code-example" markdown="1">

- 빠른 웹서비스와 보안을 위해 uwsgi 와 nginx 를 셋팅  
- uwsgi ini 파일 생성하여 실행한다  
- 아래 설정 파일 중 uid / gid 의 deploy 는 www-data 로 변경 가능  
nginx 의 기본 유저는 www-data 임

</div>

---

<div class="code-example" markdown="1">

ubuntu 에서 실제 서비스할 설정 파일 deploy.ini

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

맥에서 디버그용 debug.ini

</div>

```
[uwsgi]
home            = mypath/myvenv
chdir           = mypath/myproject
socket          = mypath/myproject/uwsgi/tmp/mysock.sock
pidfile         = mypath/myproject/uwsgi/tmp/mypid.pid
module          = server.wsgi.debug
chmod-socket=666
```

<div class="code-example" markdown="1">

- 로컬 저장 위치는 mypath/myproject/uwsgi 에 저장하면 편함

- 서비스용 uwsgi.ini 파일은 혹은 파일들은 한 곳에 모아둔다  
홈 디렉토리에 uwsgi/sites 같은 포러가 있으면 접근이 쉽고  
uwsgi emperor 를 위해 한 곳에 모아둔다  

</div>

<div class="code-example" markdown="1">
가상환경에서 uwsgi 를 실행하여 확인할 수 있다

웹페이지에서 바로 확인할려면 
ini 파일에  http = :8000 추가한다
localhost:8000 으로 웹페이지로 들어간다  
</div>

```
uwsgi -i debug.ini
uwsgi -i deploy.ini 
```

![image](/assets/images/n/1.jpeg)  

---


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

## uwsgi emperor systemd 등록

시스템에 등록하여 OS 재시작시도 실행되며 사이트 추가시에  
지정한 폴더에 들어 있는 uwsgi 파일들을 읽어 한번에 관리해줌

서비스에 등록

- 파일이름은 uwsgi.service
- 저장 경로는 /etc/systemd/system


```
sudo vi /etc/systemd/system/uwsgi.service
```

```
[Unit]
Description=uWSGI Emperor service
After=syslog.target

[Service]
ExecStart=[uwsgi 설치된 virtualenv 경로]/bin/uwsgi  --emperor [uwsgi 파일들이 있는 곳 경로]

Restart=always
KillSignal=SIGQUIT
Type=notify
StandardError=syslog
NotifyAccess=all

[Install]
WantedBy=multi-user.target 
```

## uwsgi service 시작

등록된 서비스의 재시작 시작 중지 

```
sudo service uwsgi restart / start / stop / reload(환경변수만)
```

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

---

## nginx sites-enabled / sites-available

위에 파일을 관리하기 편한 곳에 두고 위 두 곳에 링크건다
파일이 있으면 덮어쓴다 -sf

```
ln -sf [conf 파일] /etc/nginx/sites-enabled/
ln -sf [conf 파일] /etc/nginx/sites-available/
```

---

## uwsgi log 확인

uwsgi 는 위에 ini 파일에서 설정한 곳으로 이동하여 보고  

설정하지 않을 경우 아래에서 확인할 수 있음

```
/var/log/uwsgi 
```

---

## nginx log 확인

```
/var/log/nginx/access.log
/var/log/nginx/error.log
```

## 구동확인

service 를 실행 놓지 않았다면

```
sudo service uwsgi start
```

nginx 를 구동하지 않았다면

```
sudo nginx 

option)
sudo nginx -s stop
sudo nginx
```

nginx 에서 conf 에 문법 오류 체크 후 실행 될 것임

## 웹페이지 확인

서버 주소를 치고 들어가면 
500 404 뭐 이런 오류나면

log 확인
