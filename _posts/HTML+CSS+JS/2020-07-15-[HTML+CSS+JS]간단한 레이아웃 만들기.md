---
layout: article
title: "[HTML/CSS/JS] 간단한 레이아웃 만들기"
tags:
  - HTML/CSS/JS
date: 2020-07-15T08:06:00-05:00
key: "[HTML/CSS/JS] 간단한 레이아웃 만들기"
---

## 간단한 레이아웃 작업

<!--more-->

- 그림<br>![](/assets/images/200715-1.png)

- HTML과 CSS를 이용해서 위와 같은 레이아웃을 작업해보자.

## 전체 소스

```html
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>Practice Layout</title>
  <style>
    header{
      text-align: center;
      margin:10px;
      border: 1px solid black;
    }
    #container{
      overflow:auto;
      margin:10px 0px;
    }
    #nav{
      margin-right: 3%;
      width: 15%;
      float: left;
      height: 350px;
    }
    #content{
      position: relative;
      border: 1px solid black;
      float: left;
      width:80%;
      height: 350px;
    }
    footer{
      border: 1px solid black;
      text-align: center;
      clear: left;
    }
    #nav ul li{
      height: 60px;
      list-style-type: none;
    }
    .text-box{
      position: absolute;
      top: 50%;
      left: 50%;
    }
  </style>
</head>
<body>
  <header><p>Header Layout</p></header>
  <div id="container">
    <div id="nav">
      <ul>
        <li>메뉴 1</li>
        <li>메뉴 2</li>
        <li>메뉴 3</li>
        <li>메뉴 4</li>
        <li>메뉴 5</li>
      </ul>
    </div>
    <div id="content">
      <div class="text-box"><p>content</p></div>
    </div>
  </div>
  <footer><p>Footer Layout</p></footer>
</body>
</html>
```

## CSS 속성

- **float** : nav와 content는 `float: left` 로 정렬
- **clear** : footer에서 `clear: left` 로 float 밑으로 깔릴 수 있도록 정렬
- **overflow** : container는 `overflow: auto`로 float 속성이 부여 된 엘리먼트를 자식으로 인지
- **list-style-type** : 리스트는 `list-style-type: none` 로 각 리스트에 대한 원표시를 제거
- **position: absolute** : "content" 라는 표시를 가운데로 content 엘리먼트 가운데로 정렬하기 위해 `position: absolute` 속성을 사용하고 `top: 50%, left: 50%` 부여
- **position: relative** : content의 `position: absolute` 속성은 부모 엘리먼트 중 `position: static`이 아닌 엘리먼트를 기준으로 정렬되기 때문에, content의 부모 엘리먼트인 container에 `position: relative` 속성 사용
