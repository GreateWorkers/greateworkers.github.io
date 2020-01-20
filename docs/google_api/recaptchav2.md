---
layout: default
title: reCaptcha v2
parent: Google API
nav_order: 1
previous: ../google_api
next: recaptchav3
---

{% include nav_btn.html %}

# Google reCaptcha v2
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 소개

나는 로봇이 아닙니다. 사이트 무단 수집 방지 등을 위해 사용  
로그인 없이 익명의 사용자가 서버에 요청을 하는 경우  
등을 방지

v2 v3 차이는 
v3는 로봇이 아닙니다 체크박스가 표시되지 않고
사용자의 행동을 점수로 로봇인지 아닌지 체크함   

v2는 체크박스와 백그라운드 둘 다 있음  


v3 [문서](https://developers.google.com/recaptcha/docs/v3)  

v2 [문서](https://developers.google.com/recaptcha/docs/display)  

---

## 구현할 내용

사이트 웹마스터의 이메일 주소를 클릭하면  
이메일이 나타나도록 한다.  


사용 서버 django
recaptcha 검증을 ajax 로 요청

---
## reCaptcha v2

방법은 아래 v3 항목에서

### reCaptcha v2 등록

구글에 계정이 있다면  

youtube recaptcha [console](https://www.google.com/recaptcha/intro/v3.html)    


'+' 클릭  
![image](/assets/images/googleapi/screenshot10.jpg)

아래 이미지 나옴   
reCaptcha v2 선택  

![image](/assets/images/googleapi/screenshot8.jpg)  

원하는 형태를 선택  
![image](/assets/images/googleapi/screenshot11.jpg)  


도메인  
자신이 보유한 도메인이 있으면 적어줌  
로컬 개발 상태이면 해당 로컬 서버 ip 주소   
ex) 127.0.0.1, 192.168...   

약관에 동의하고 저장  

---
### 사이트키/비밀키 
그러면 사이트키 / 비밀 키가 나옴
  
- 사이트키는 html javascript에서 api에 파라미터로 전송하여  
- 어떤 사이트인지 api 시작을 요청하고  
- api가 준비되면 토큰을 발급해준다.
비밀키 검증을 요청한다  

```
requests.post('https://www.google.com/recaptcha/api/siteverify', data=data)#비밀키
```
---
### html 구현
      
```
<!-- 구글 api 호출 -->
<script src="https://www.google.com/recaptcha/api.js" async defer>
</script>

<!-- 사이트키 설정 -->
<div id="captcha" class="g-recaptcha" data-sitekey="site-key"></div>

<!-- jquery에서 클릭이벤트 받을 링크 -->
<a id='webmaster_mail' href="javascript:;">mailto</a>

<!-- 장고 csrf -->
<input id="csrf_token" type="hidden" value="{{csrf_token}}">

<!-- 토큰이 만들어지면 여기 value에 들어감 -->
<input type="hidden" id="g-recaptcha" name="g-recaptcha-response">

```

---
### 장고 구현


아래 decorator 소스 [출처!](https://simpleisbetterthancomplex.com/tutorial/2017/02/21/how-to-add-recaptcha-to-django-site.html)  


```
def check_recaptcha(view_func):
    @wraps(view_func)
    def _wrapped_view(request, *args, **kwargs):
        request.recaptcha_is_valid = None
        if request.method == 'POST':
            recaptcha_response = request.POST.get('g-recaptcha-response')
            data = {
                'secret': settings.GOOGLE_RECAPTCHA_SECRET_KEY,
                'response': recaptcha_response
            }
            r = requests.post('https://www.google.com/recaptcha/api/siteverify', data=data)
            result = r.json()
            if result['success']:
                request.recaptcha_is_valid = True
            else:
                request.recaptcha_is_valid = False
                messages.error(request, 'Invalid reCAPTCHA. Please try again.')
        return view_func(request, *args, **kwargs)

    return _wrapped_view
```


ajax를 통해 호출할 곳에 decorator를 사용

```
@check_recaptcha
def request_webmaster_mail(request):
    if request.method == 'POST':
        if request.recaptcha_is_valid:
            return HttpResponse(json.dumps({ 'message': 'greateworkers@gmail.com'}))
        else:            
            return HttpResponse(json.dumps({ 'message': ''}))

    return HttpResponse(json.dumps({'message': ''}))

```


### ajax 구현

```

$(function(){    
    $('#webmaster_mail').click(function(){
        if (grecaptcha.getResponse() == "")
        {
            $('#captcha').show();
            return;
        }
        $.ajax({
            type: "POST",
            url: "/webmaster/mail",
            data: {
            'g-recaptcha-response': grecaptcha.getResponse(),
            'csrfmiddlewaretoken':$('#csrf_token').val(),
            },
            dataType: "json",

            success: function(response){
                alert('ok');
            },

            error: function(request, status, error){
                alert('error');
            },
        });
    });    
});
```



{% include nav_btn.html %}