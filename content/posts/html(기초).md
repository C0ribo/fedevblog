---
title: 시작 html
date: 2023-05-12
tags: [html, study]

---
# Basic

```html
<!DOCTYPE html>
<!-- 현재 작성하는 버전이 몇버전인지 명시 -->
<html lang="ko">
<!-- 언어를 표시, ko는 한글로 작성표시. 검색엔진에서 한글 자료 찾기 쉬워짐 -->
  <head>
    <meta charset="UTF-8" />
    <!-- 현존하는 모든 언어가 깨지지 않고 나옴 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge" /> 
    <!-- 익스플러의 최신버전을 말함 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" /> 
    <!-- 모바일 관련 -->
    <title>Document</title>
  </head>
  <body>
    
  </body>
</html>
```

# 기본 설명

* **!DOCTYPE html**
* **html lang="en"**
* **meta charset="UTF-8"** 
* **meta http-equiv="X-UA-Compatible" content="IE=edge"** 
* **meta name="viewport" content="width=device-width, initial-scale=1.0"** 
* **<title>Document</title>** 문서의 제목을 나타내는 title 태그

# Element

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Document</title>
  </head>
  <body>
    <h1>머리말1</h1>
    <h2>머리말2</h2>
    <h3>머리말3</h3>
    <h4>머리말4</h4>
    <h5>머리말5</h5>
    <h6>머리말6</h6>

    <p>첫번째 문장입니다</p>
    <!-- 일반 텍스츠로 문장 하나하나 만드는 것과 같음 -->
  </body>
</html>
```
<!-- ![글씨크기]( ) -->

# attribute

```html
  <a href="https://www.naver.com/">네이버</a> 
  <!--속성이라고 한다.반드시 시작태그에서만 쓸 수 있다. -->
  <a href="./01_helloworld.html">01_helloworld.html</a> 
  <!--화면 이동을 담당하고있는건 a태그 -->
```

