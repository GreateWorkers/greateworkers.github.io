---
layout: default
title: postgres 
nav_order: 3
parent: Lightsail 웹서비스 시작하기
---

# postgres 설치
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

[이전 단계](../Lightsail_source_2) |
[다음 단계](../Lightsail_uwsgi_nginx_4)

## 소개

데이타베이스를 설치한다. 

---

## postgresql 설치

<div class="code-example" markdown="1">

postgresql 을 설치

</div>

```
sudo apt-get install postgresql postgresql-contrib libpq-dev
```

---

## 데이타베이스 생성

<div class="code-example" markdown="1">

설치 후 postgres 터미널로 접속  
접속 데이타베이스 / 인코딩 / 관리자 생성

</div>


```
sudo su - postgres
psql

CREATE USER [관리자이름] WITH PASSWORD [password];

ALTER ROLE [관리자이름] SET client_encoding TO 'utf8';
ALTER ROLE [관리자이름] SET default_transaction_isolation TO 'read committed';
ALTER ROLE [관리자이름] SET timezone TO 'UTC';

CREATE DATABASE [database_name] OWNER [owner_name];

\q
ctrl+D
```

---

## 데이타베이스 외부접속 

<div class="code-example" markdown="1">

로컬 ip를 쉽게 찾아서 postgres 에게 원격 db 연결을 허용한다  
[findip](http://www.findip.kr)

</div>

<div class="code-example" markdown="1">

pg_hba.conf 의 경로 중 버전은 설치한 것에 따라 변경

</div>

```
sudo vi /etc/postgresql/9.5/main/pg_hba.conf
```

<div class="code-example" markdown="1">

pg_hba.conf 파일을 아래로 계속 내리면  
all all 이런 식의 표현들 사이에 로컬 주소를 추가해준다.
</div>

```
host [데이타베이스이름] [관리자이름] [로컬 ip]/32 md5
```
 
<div class="code-example" markdown="1">

그리고 한가지 더 변경해 주어야 한다  
파일을 열어 조금 내리면 listen_address 항목이 나옴
허용을 모두해준다. 

</div>

```
sudo vi /etc/postgresql/9.5/main/postgresql.conf

listen_addresses='*'
```

<div class="code-example" markdown="1">
postgres 재시작
</div>

```
 sudo /etc/init.d/postgresql restart
```

---

## 로컬 데이타베이스를 덤프하기

<div class="code-example" markdown="1">

pg_dump 파일을 실행하여 데이타베이스 이름을 지정하여 sql 로 생성  

문구 오류가 있을 수 있어서 sed 로 변경
</div>

```
pg_dump [데이타베이스 이름] > ws.sql
sed 's/AS integer//' ws.sql > ws_dump.sql
```

<div class="code-example" markdown="1">

서버에 덤프하고 잘 들어갔는지 확인까지

테이블 리스트를 보면서 확인 할 수 있음

</div>

```
sudo su - postgres
psql -d [데이타베이스 이름] -f ws_dump.sql

psql
\l
\connect [데이타베이스 이름]
\dt *
(space bar)
\q
ctrl+D
```


[이전 단계](../Lightsail_source_2) |
[다음 단계](../Lightsail_uwsgi_nginx_4)
