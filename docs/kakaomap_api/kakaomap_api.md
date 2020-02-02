---
layout: default
title: Kakaomap API
nav_order: 8
has_children: true
permalink: /docs/kakaomap_api
---

# Kakaomap api

카카오지도 샘플이 너무 잘 되어 있음

클러스터 기능은 좌표 정보로 마커를 전부 생성해야 함  

아래 부분은 헤더에 추가하여 저렇게 보내니 됨    
query 부분은 메뉴얼에 나온 url을 적으면 됨  

python request
```
response = requests.get(query, headers={'Authorization': f'KakaoAK {KAKAOMAP_KEY}'})

js = json.loads(response.text)
```

끝

{: .fs-5 .fw-300 }