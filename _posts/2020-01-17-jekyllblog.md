---
layout: post
title: "Jekyll을 이용한 github 블로그 만들기"
date: 2020-01-17 13:12:00
description: Jekyll을 이용한 github 블로그 만들기
tags: Jekyll
comments: true
---

# Jekyll을 이용한 github 블로그 만들기

github.io의 주소를 가지는 github블로그 만들기

## github repository 생성하기

github의 블로그를 만들기 위해서는 먼저 블로그 repository를 생성해야 합니다.  
하단 그림의 빨간 박스에 있는 `new`버튼을 눌러 새로운 repository를 생성합니다.

![](/../image/makerepository.png)

Repository의 이름을 `블로그이름`.github.io로 생성을 해줍니다. 이 Repository의 이름이 블로그의 주소가 됩니다.
예를 들어 제 블로그를 생성할때 Repository의 이름을 `msc9533.github.io`으로 생성을 했기 때문에 `https://msc9533.github.io`가 블로그의 주소가 됩니다.  
또 Initialize this repository with a README의 옵션을 체크해 주어 하단의 `Create repository`를 누르면 쉽게 블로그를 생성할 수 있습니다.

![](/../image/blogurl.png)

repository를 생성한 후 로컬 저장소에서 clone을 진행 해 줍니다.

## Jekyll 테마 적용하기

### Jekyll이란?

markdown, HTML 등의 언어로 작성된 글을 정적 웹사이트로 변환해 업로드 할 수 있게 해주는 사이트 개발 툴이다. 정적페이지기 때문에 속도가 빠르고 github에 업로드 하는 경우 Jekyll과 Ruby를 굳이 설치 하지 않아도 됩니다.

### 테마 적용

먼저 이 [페이지](http://jekyllthemes.org/)에서 마음에 드는 테마를 선택합니다. 저는 jekyll-clean-dark라는 이름의 테마로 선택 했습니다.

---
참고문서
- [Jekyll을 이용해 GitHub에 블로그 만들기 (1)](https://jetalog.net/86)
- [Jekyll을 이용하여 Github Blog 만들기](http://jinyongjeong.github.io/2017/01/08/jekyll_blog_making_new/)
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>
