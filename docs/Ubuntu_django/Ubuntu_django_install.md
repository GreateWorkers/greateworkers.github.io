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

pip을 업그레이드합니다.  
sudo -H pip3 install --upgrade pip  

가상환경을 설치합니다.  
sudo -H pip3 install wheel    
sudo -H pip3 install virtualenv   
 
uwsgi는 pip을 통해 가상환경에 설치.  
uwsgi를 관리하는 방식에 따라 아래와 같이 할 수 있으나 Django 가상환경에 pip으로 설치하여 사용  
sudo -H pip3 install uwsgi  

---

## virtualenv 설정
파이선의 버전을 지정하여 가상환경을 설치.  
차후 pip 패키지 설치 시 파이선 버전오류가 발생하지 않음.    
 
가상환경 설치    
virtualenv --python=python3.6 myvenv  

가상환경 실행  
. myvenv/bin/activate  

---

## django 설치

pip install django  

장고에 webserver 프로젝트 생성
django-admin startproject webserver    

---
 
## uwsgi 설정 파일

## nginx 설정 파일

## 오류 메세지
