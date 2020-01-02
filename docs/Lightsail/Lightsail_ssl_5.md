---
layout: default
title: ssl
nav_order: 5
parent: Lightsail 웹서비스 시작하기
---

# Lightsail 에 웹서비스
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
[이전 단계](../Lightsail_uwsgi_nginx_4) |
[다음 단계](../Lightsail_error_6)

## ssl certbot

ssl 을 사용하기 위해서 아래의 명령어를 사용하여 간단하게 해결한다  
여러 도메인을 사용할 경우 -d [사이트명] 으로 나열한다

```
sudo certbot --nginx  -d www.greateworkers.kr -d greateworkers.kr -d petitions.greateworkers.kr
```

명령어를 입력하면  
몇 가지 질문 사항이 있음

- 갱신할 것인지 새로할 것인지 물어봄(처음이 아닐경우)
- https redirection 할 것인지
- certbot 을 만든 곳에서 어쩌구 하니 할 것인지 물어봄 
- 인증서 테스트 사이트 링크도 알려줌  

자주 새로 인스톨 하면 몇 일간 몇번 이상은 안된다고 메세지 던짐  
신규 설치 후에는 갱신을 선택

인증서 테스트 사이트 

아래 d=[발급받은 사이트명]  
```
https://www.ssllabs.com/ssltest/analyze.html?d=www.greateworkers.kr
```


[이전 단계](../Lightsail_uwsgi_nginx_4) |
[다음 단계](../Lightsail_error_6)
