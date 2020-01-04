---
layout: default
title: 설치 Jekyll
parent: Github blog
nav_order: 2
previous: install_Folk_1
next: install_Local_addgit_3
---

{% include nav_btn.html %}

# 설치 Jekyll
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 소개

jekyll 로컬 서버와 연동하여 블로그를 만듭니다.<br>
github에 올려 서비스합니다.<br><br>

저는 맥 osx mojave 10.14 입니다.<br>
구형 맥북으로 macbook 13inch 2010 unibody 입니다.<br>

---

## jekyll

루비! 설치하는데 다소 시간이 걸렸지만 오류는 없었습니다.<br>
테마에 설정시에도 큰 문제는 없었습니다.<br>


---

### rvm 및 루비 설치

아래를 통해 설치한 루비 버전입니다.<br>
ruby -v<br>
ruby 2.5.0p0 (2017-12-25 revision 61468) [x86_64-darwin18]<br>
<br>

<div class="code-example" markdown="1">
rvm 설치합니다. <small>rvm 관련 링크 <a href="https://rvm.io">rvm</a></small><br>
루비 설치합니다. <small>루비 버전 확인 <a href="https://www.ruby-lang.org/ko/downloads/">ruby</a>
</small>
</div>

```
\curl -sSL https://get.rvm.io | bash -s stable<br>
rvm --default use 2.5.0
sudo gem install jekyll bundler
```
<br>

---

### jekyll 프로젝트 폴더 생성


<div class="code-example" markdown="1">
적당한 폴더 위치로 이동합니다.<br>
폴더를 생성학고 아래 명령어를 실행합니다.
</div>

```
jekyll new greateworkers.github.io
cd greateworkers.github.io
ls
404.html	Gemfile.lock	_posts		index.markdown
Gemfile		_config.yml	about.markdown
```

<br>



## jekyll server 실행


<div class="code-example" markdown="1">
jekyll 로컬 서버 실행합니다.
</div>
{% highlight markdown %}
jekyll serve
{% endhighlight %}
<br>


<div class="code-example" markdown="1">
jekyll 포트를 바꿀 수 있습니다.
</div>
{% highlight markdown %}
jekyll serve --port 4001
{% endhighlight %}
<br>

<div class="code-example" markdown="1">
jekyll serve 실행 결과 입니다.
</div>

```
Configuration file: /mypath/greateworkers.github.io/_config.yml
            Source: /mypath/greateworkers.github.io
       Destination: /mypath/greateworkers.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
       Jekyll Feed: Generating feed for posts
                    done in 1.356 seconds.
 Auto-regeneration: enabled for '/mypath/greateworkers.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```
<br>


## 브라우저로 확인
browser 열고 127.0.0.1:4000 (default) 입력합니다.
![image](/assets/images/Pasted Graphic.jpeg)

---

## 오류
fsevent: running worker failed: incompatible character encodings: ASCII-8BIT and UTF-8

대분류에 한글 쓰니 나타나는 오류

{% include nav_btn.html %}