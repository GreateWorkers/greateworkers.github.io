---
layout: default
title: reCaptcha v3
parent: Google API
nav_order: 2
previous: recaptchav2
next: youtube_search
---

{% include nav_btn.html %}

# Google reCaptcha v3
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 소개

v2와 거의 동일
Html 부분이 다름  

---

## 구현할 내용

사이트 웹마스터의 이메일 주소보기를 클릭하면  
이메일이 나타나도록 한다.  


사용 서버 django
recaptcha 검증을 ajax 로 요청

---
### 장고 구현

v2와 동일  

---
### html 구현

```
<script src="https://www.google.com/recaptcha/api.js?render={{recaptcha_sitekey}}"></script>
```


```
<input type="hidden" id="g-recaptcha" name="g-recaptcha-response">
<input type="hidden" id="recaptcha-action" value="[구분할 이름]">
```

recaptcha.js
js 파일로 만들어서 hidden 값을 불러오는 것을 적어줌  

```
$(function(){
    
    grecaptcha.ready(function() {
        var sitekey = $('#recaptcha-sitekey').val();
        var action = $('#recaptcha-action').val();
        grecaptcha.execute(sitekey, {action: action}).then(function(token) {
            
            $('#g-recaptcha').val(token);

        });
    });
    
});
```


---
### 약관 표시 아이콘

v3 는 사이트에서 안내하는데로 할 경우  
우측 하단에 저런 아이콘이 표시됨 

![image](/assets/images/googleapi/screenshot9.jpg)

그냥 없애면 안되고 개인정보 사항에 대한 고지를 해야함  
왜냐면 사용자 행동을 수집해서 결과를 내기 때문인 것 같음  

```
.grecaptcha-badge { visibility: hidden; }
```

그리고 
이 문구를 표시해야함  


```
This site is protected by reCAPTCHA and the Google
    <a href="https://policies.google.com/privacy">Privacy Policy</a> and
    <a href="https://policies.google.com/terms">Terms of Service</a> apply.
```

참고 문서 [Google FAQ](https://developers.google.com/recaptcha/docs/faq#id-like-to-hide-the-recaptcha-badge.-what-is-allowed)


{% include nav_btn.html %}
