---
layout: article
title: "[HTML/CSS/JS] DOM Tree를 사용한 HTML element 탐색.md"
tags:
  - HTML/CSS/JS
date: 2020-07-21T08:06:00-05:00
key: "[HTML/CSS/JS] DOM Tree를 사용한 HTML element 탐색.md"
---

## DOM Tree를 사용한 HTML element 탐색.md

DOM Tree를 알아보고, 이를 활용한 HTML 탐색 방법을 알아보자.<br>

<!--more-->

### DOM(Document Object Model) Tree

- DOM Tree <br>![](/assets/images/200721.png)<br>출처 : [w3schools](https://www.w3schools.com/js/js_htmldom.asp)

HTML로 이루어진 코드가 브라우저 상에서 작동하면, 브라우저는 HTML 요소들을 **DOM (Document Object Model) Tree** 의 형태로 위와 같은 객체 구조로 저장을 하고, 이 구조가 Tree 구조이기 때문에 DOM Tree라고 부른다.<br><br>
위의 그림에서 알 수 있듯이 가장 최상단인 부모노드에는 Document로 이루어져있고, JavaScript로 HTML의 요소들을 찾아서 이용하기 위해 만들어진 메소드들이 이 DOM 구조를 기반으로 탐색을하여 이용할 수 있도록 만들어졌다.<br>

### DOM Tree 조작을 위한 메서드(DOM API)

- getElementById()
  - 해당 HTML의 요소 중 아이디를 통해 찾는 것을 지원해주는 API이다.
  - Document가 DOM Tree의 최상단이므로, `document.getElementById("찾는 ID")` 의 식으로 원하는 ID의 HTML 요소를 찾을 수 있다.
- Element.querySelector()
  - css를 작성할 때 사용하는 `css selector`를 이용한 문법으로 찾아주는 메서드이며, 동일한 css selector의 요소들을 모두 출력해주는 `querySelectorAll`도 있다.
