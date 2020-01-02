---
layout: default
title: Clone source
nav_order: 2
parent: Lightsail 웹서비스 시작하기
---

# Lightsail 에 웹서비스
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
[이전 단계](../Lightsail_install_1) |
[다음 단계](../Lightsail_postgres_3)

## Git 에서 Source 를 pull
<div class="code-example" markdown="1">

http 주소로 clone

지메일로 가입하였다면  
app password을 설정하면 패스워드처럼 사용가능

</div>

```
git config --global credential.helper 'cache --timeout=[시간 설정가능]'
git clone https://~
git clone https://~
```

---

## pip 으로 requirements.txt 패키지 설치
<div class="code-example" markdown="1">

프로젝트 개발하면서 설치한 패키지를 설치

</div>

```
pip install -r requirements.txt
```

<div class="code-example" markdown="1">

비밀키 등을 업로드 한다  
사용한 프로그램은 osx FTP 프로그램 FolkLift  

- FTP 포트 개방은 lightsail 관리 > 네트워크 > 규칙설정
- ssh 키 또한 FolkLift 에 등록하여 사용

</div>


[이전 단계](../Lightsail_install_1) |
[다음 단계](../Lightsail_postgres_3)