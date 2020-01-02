---
layout: default
title: 설치 Folk github
parent: Github blog
nav_order: 1
---

# Folk 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 소개

적당한 소스 찾아서 바로 소스를 가져와 서비스를 시작하여 Github 블로그를 체험

Github 페이지에서 글자 바꿔가며 테스트하고 감 잡을 수 있어서 좋은 듯

---
## 블로그 소스 찾기

간단한 블로그 소스를 찾습니다
https://jekyllthemes.io/free

![image](/assets/images/11_02_06.jpeg)

github 저장소로 들어갑니다.
![image](/assets/images/11_05_28.jpeg)

---

## 소스 Folk

소스를 Folk 하여 저장소 설정을 합니다.

setting 버튼을 클릭하여 들어갑니다.
![image](/assets/images/11_08_29.jpeg)

저장소 이름을 지정합니다. 
Repository name

[본인 아이디].github.io 로 설정합니다.

저런식으로 주소를 입력하고 들어올 수 있는 것은 하나만 지정할 수 있습니다.
저는 greateworkers.github.io 로 지정했습니다.
![image](/assets/images/11_11_10.jpeg)

---

## 서비스 시작

잠시 기다렸다가 [본인 아이디].github.io 주소창에 입력하여 확인합니다.



---

## 도메인 연결

보유한 도메인에 설정하고 싶은 경우 설정합니다.
화면을 좀 더 내리면 아래 설정이 나옵니다.
![image](/assets/images/11_14_49.jpeg)

custom domain 에 보유 도메인을 입력합니다.
본인 사이트는 https를 강제 리다이렉션이 되어있습니다. 그래서 아래 https 리다이렉션도 체크했습니다.


---

## Robots.txt


<div class="code-example" markdown="1">
robots.txt를 작성하여 크롤링 범위를 설정합니다.
우선 아래와 같이 작성하여 메인에(root 폴더에) 추가합니다.

안해도 됨
</div>
```
User-agent: *
Disallow: /
```
