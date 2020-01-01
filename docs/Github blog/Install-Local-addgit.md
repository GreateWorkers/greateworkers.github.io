---
layout: default
title: Github ssh key 등록 후 push
parent: Github blog
nav_order: 3
---

# Github push ssh key 등록하기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 소개

jekyll을 로컬에 프로젝트 설치한 상태에서 Github에 올리기

---

## jekyll Github에 올리기

Folk 라는 프로그램을 사용하여 github에 저장소를 생성하거나 사이트에서 생성
잘 몰라도 Folk 가 무지 편하네.

![image](/assets/images/3.19.59.png)

그림의 ssh 탭을 클릭하면 ssh 키를 생성하여 사용할 수 있다.

![image](/assets/images/3.22.02.png)

---
## Github ssh key 등록하기

Folk 로 sshkey 생성해서 추가한다.

![image](/assets/images/3.36.00.png)

그리고 ssh 키를 Github 사이트에 자동 등록해주던 것 같은데. 
만약 안되면 Github 계정 프로파일에 들어가서 생성한 키를 등록해준다

![image](/assets/images/3.46.45.png)

---
## Github 저장소 생성

그리고 github에 저장소를 생성하였다.

![image](/assets/images/3.51.06.png)

---
## Github 저장소 checkout

생성한 저장소를 아무곳에 clone 한다<br>
생성한 저장소의 .git 폴더를 복사해서 jekyll 폴더로 옮긴다.<br>
Folk 프로그램에서 jekyll 폴더를 open 해주면 된다<br>
clone 할 때 덮어쓰기가 된 것 같기도 한대.<br>

![image](/assets/images/3.51.44.png)
