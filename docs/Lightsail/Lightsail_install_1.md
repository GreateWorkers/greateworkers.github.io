---
layout: default
title: Lightsail service
nav_order: 1
parent: Lightsail 웹서비스 시작하기
next: Lightsail_source_2
---
{% include nav_btn.html %}

# Lightsail 웹서비스 시작하기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


## 소개

Lightsail 가입하고 패키지 설치 / 터미널 접속까지

---

## Lightsail 가입

![image](/assets/images/s2.jpeg)

[lightsail](http://lightsail.aws.amazon.com){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

-   가장 저렴한 것이 \$3

---

## 인스턴스 생성

-   lightsail 에 가입하여 인스턴스를 생성
-   os 는 ubuntu lts 18
-   ubuntu lts 16 을 설치해도 python 3.6 쉽게 깔리고 사용이 잘됨

![image](/assets/images/s3.jpeg)

---

## 터미널로 접속

-   터미널 접속을 위한 키 생성  
    관리 선택  
    ![image](/assets/images/s1.jpeg)

<div class="code-example" markdown="1">

터미널 접속

</div>
 
```
ssh -i [ssh 키 파일].pem [사용자계정]@[ip] -o "StrictHostKeyChecking no"
```

---

## 웹터미널 이용

웹터미널을 이용하면 키생성 없이 바로 접속  
웹은 세션 시간이 짧아 불편

---

## 패키지 설치

<div class="code-example" markdown="1">
[timezone]  
</div>

```
sudo dpkg-reconfigure tzdata
```

<div class="code-example" markdown="1">
[package]  
</div>

```
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-add-repository ppa:webupd8team/java
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3.6 python3.6-dev python3-pip
sudo apt-get install python3-setuptools
sudo apt-get install python3-dev
sudo apt-get install python-certbot-nginx
sudo apt-get install expect
sudo apt-get install nginx
sudo -H pip3 install --upgrade pip
sudo -H pip3 install wheel
sudo -H pip3 install virtualenv
sudo -H pip3 install uwsgi
```

<div class="code-example" markdown="1">

자바 라이센스가 변경되어 인스톨러는 더이상 무료로 제공하지 않음  
수동으로 설치

</div>

```
sudo apt-get install oracle-java8-installer (x)
```

<div class="code-example" markdown="1">
[virtualenv]
</div>

```
mkdir mysite
virtualenv --python=python3.6 myvenv
. myvenv/bin/activate
```

## 오류

ERROR: Command errored out with exit status 1: /usr/bin/python3 -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-kfs97u50/uwsgi/setup.py'"'"'; **file**='"'"'/tmp/pip-install-kfs97u50/uwsgi/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(**file**);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, **file**, '"'"'exec'"'"'))' install --record /tmp/pip-record-bd067dgu/install-record.txt --single-version-externally-managed --compile Check the logs for full command output.

{% include nav_btn.html %}