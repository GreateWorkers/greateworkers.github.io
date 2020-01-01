---
layout: default
title: virtual machine 에 ubuntu 설치
nav_order: 1
parent: Ubuntu django 설치
---

# Virtual Machine 에 ubuntu 설치
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 소개

virtual machine 을 ubuntu 설치하고 nginx + uwsgi + django project 실행
ubuntu 를 터미널로 접속하여 사용하면 cpu 가 저사양이라 다소 버벅임

그리고 virtual machine 에서 ubuntu를 사용하지 않는다.<br>
터미널로 접속하여 사용

---

## 설치 환경
2.4 intel core 2 duo<br>
macbook pro 13inch 2010 <br>
osx 10.14 mojave <br>
16g ram<br>

---

## Virtual Machine 설치

- Virtualbox download <br>
https://www.virtualbox.org
 
- ubuntu lts download<br>
http://releases.ubuntu.com


ubuntu lts 18 선택


---


## Virtualbox 네트워크 설정
설치 이미지 설정 > 네트워크 탭 > 어댑터에 브리지 선택 > 재부팅
VM 에서 다운 받은 ubuntu OS 세션을 생성하고 이미지를 선택하여 설치를 진행한다.

---

## ubuntu 설치

우분투 설치시 몇 번 실패하였다.
기본 옵션만 선택하니 잘 되지 않아 

중간에 kr.archive.ubuntu.com/ubuntu > us.archive.ubuntu.com/ubuntu 을 변경

파티션 선택 항목에서 
기본 entire disk 을 entire disk and LVM 이라는 항목으로 변경

curtin > LVM 으로 설치
마지막에 업데이트 하는데 그냥 끄고 재부팅

---

## Virtual Machine 에 설치된 ubuntu terminal 접속 
 

- 설치 후 조금 기다렸다가
 

- ifconfig 실행
   상세내용이 출력 됨 > inet addr 에 ip 확인 ( 공유기에서 할당받은 ip가 뜸 )
 

- terminal 로 접속
ssh ubuntu@192.168.x.x
 

- yes/no 질문에 yes

---


## ubuntu kakao 로 다운로드 링크 변경

sudo vi /etc/apt/sources.list
파일 내에 있는 모든 저장소 주소를 mirror.kakao.com으로 변경해주면 된다.

%s/(변경할 대상)/(변경할 값) 명령어를 사용.
:%s/kr.archive.ubuntu.com/mirror.kakao.com

패키지 업데이트
sudo apt-get update 
 

---