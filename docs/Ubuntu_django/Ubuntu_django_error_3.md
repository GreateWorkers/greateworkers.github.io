---
layout: default
title: error 메세지
nav_order: 3
parent: Ubuntu django 설치
previous: Ubuntu_django_install_2
next: ../Ubuntu_django
---

{% include nav_btn.html %}

# error 메세지
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## error 메세지

![image](/assets/images/n/3.jpeg)  

uwsgi 에서 발생하는 오류 log   
nginx 의 확인할 수 있는 access.log error.log 등은 아래와 같다

- permission  
- access  
- file or directory not found  

오류는 경로를 확인한 후 이상이 없다면  
계정을 통일하지 않고 또 폴더 권한 설정이 되지 않은 경우다

접속 계정 a로 서비스용 b계정을 만들었고 (예) deploy,www-data 등)
실제 작업은 a로 하기 때문에 a가 만든 tmp 같은 폴더는 권한 바꿔줘야 함   

uwsgi 에 설정한 폴더 중 삭제나 추가 등의 권한이 필요한 것은  
서비스용 b계정이 폴더 소유자가 되어야 한다 (예) tmp)

{% include nav_btn.html %}