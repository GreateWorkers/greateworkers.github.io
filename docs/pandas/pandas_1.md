---
layout: default
title: Lightsail service
nav_order: 1
parent: Lightsail 웹서비스 시작하기
previous: ../Pandas
next: Lightsail_source_2
---
{% include nav_btn.html %}

# Pandas 시작
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


## pandas

데이터 분석하기 위한 라이브러리

---

## 강좌 듣고 따라하기

동영상 강좌들으며 시작

---

## 설치
 pycharm 에 jupyter notebook 설치
 필요 라이브러리 설치
 
---

## pandas 의 속성
data frame 
serial
...
---

## 코드

```
# import requests
# from bs4 import BeautifulSoup as bs
import pandas as pd
import numpy as np
import re
import matplotlib.pyplot as plt
# from tqdm import trange

%matplotlib inline
%config InlineBackend.figure_format = 'retina'


plt.rc("font",family="AppleGothic")
plt.rc("axes",unicode_minus=False)

df = pd.read_csv("clien-reply.csv")
df.shape


df.shape
df = df.drop_duplicates(["text"],keep="last")

 


keywords = ["미국","한국","한정","사용"]
for keyword in keywords:
    df[keyword] = df["text"].str.contains(keyword)
    

    
df_python = df[df["text"].str.contains("미국")].copy()
df_python.tail()



df[keywords].sum().sort_values(ascending=False)


df.loc[(df["사용"] == True),"text"]



```
---

```text

import requests
from bs4 import BeautifulSoup as bs
import pandas as pd

from tqdm import trange


base_url = "https://www.clien.net/service/board/park/14544635?type=recommend"
response = requests.get(base_url)
soup=bs(response.text,'html.parser')


#soup
replys = []
content = soup.select("#div_content > div.post_comment > div.comment > div")
content_length = len(content)
# print(content[118])
# print(content[59].select("div.comment_view")[0].get_text(strip=True))
for i in trange(content_length):    
    reply = content[i].select("div.comment_view")
    if len(reply) > 0:
        replys.append(reply[0].get_text(strip=True))
    else:
        replys.append("삭제되었습니다.")
#div_content > div.post_comment > div.comment > div:nth-child(2)


df = pd.DataFrame({"text":replys})
df.shape


df.to_csv("clien-reply.csv",index=False)

```


{% include nav_btn.html %}