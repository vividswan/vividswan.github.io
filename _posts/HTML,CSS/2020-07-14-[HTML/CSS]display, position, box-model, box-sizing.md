---
layout: article
title: "[HTML/CSS] CSS layout 기능"
tags:
  - HTML/CSS
date: 2020-07-14T08:06:00-05:00
key: "[HTML/CSS] CSS layout 기능"
---

## CSS layout 작업

엘리먼트를 화면에 배치하는 것

<!--more-->

## display

- **block** : 기본 속성, 위에서 아래로 쌓음
- **inline** : 왼쪽에서 오른쪽으로 쌓임(꽉 차면 오른쪽 아래로 쌓임, span, a, strong ...)
- **inline-block** : inline의 속성에서, 넓이와 배치 등의 옵션을 적용할 수 있음

## position

- **static** : 순서대로 배치
- **absolute** : 기준점(상위 엘리먼트 중 static이 아닌 position, 상위 엘리먼트가 모두 static이라면 body가 기준)에 따라 위치(top, left 값을 주는 것이 좋음)
- **relative** : 원래 자신이 위치하여야 할 곳을 기준으로 상대적인 곳
- **fixed** : 스크롤 같은 것에 움직이지 않는 고정된 레이어

## float

- **left** : 좌측 끝에 배치
- **right** : 우측 끝에 배치
- **clear** 속성을 적용하면, float를 지정해 주었던 효과가 사리지게 됨

## box-model

- **margin** : 엘리먼트 간 간격
- **border** : 테두리 정보(두께)
- **padding** : 엘리먼트 안에 콘텐츠와 그 엘리먼트가 가지고 있는 크기 사이의 간격

## box-sizing

- **border-box** : 내부의 엘리먼트 크기를 고정하면서 부모 엘리먼트의 padding 값을 늘릴 수 있음
