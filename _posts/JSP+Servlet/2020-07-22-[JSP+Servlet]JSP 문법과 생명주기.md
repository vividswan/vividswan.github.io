---
layout: article
title: "[JSP/Servlet] JSP 문법과 생명주기"
tags:
  - JSP/Servlet
date: 2020-07-22T08:06:00-05:00
key: "[JSP/Servlet] JSP 문법과 생명주기"
---

# JSP의 특징

<!--more-->

- **JSP의 위치** : JSP는 프로젝트 파일 중 `WebContent` 파일 아래에 생성된다. (WebContent에는 HTML, JavaScript 파일 등도 저장한다.)

- **Servlet으로 바뀌어서 동작** : JSP는 그 자체가 JSP로 실행되는 것이 아니라 Servelt으로 바뀌어서 동작하는 것이기 때문에, Servlet과 생명주기가 똑같고, JSP의 문법이 Servlet에서 어떻게 바뀌어서 동작하는지 파악하는 것이 중요하다.

- **실행순서** : 클라이언트에서 최초로 JSP를 요청 -> Java 코드로 변환 -> 컴파일(~.class 파일 생성) -> 클래스를 로딩하고 인스턴스를 생성

## JSP 주요 문법

- **<%@ %>** : 지시자, 페이지에 미리 정의되는 내용을 입력하며, 보통 WAS가 읽음 (<%@ page .. %>는 page 지시자)

- **<% %>** : Scriptlet라고 하며, 실행시킬 자바 코드를 입력하는 부분 (<% %> 을 제외하고 그대로 자바 코드로 변함)

- **<%= %>** : 표현식, Servlet의 out.print()(`PrintWriter out = response.getWriter();`) 와 동일하여 변숫값을 클라이언트에게 바로 출력할 수 있다.

- **<%! %>** : 선언식, 클래스에 메서드나 필드를 선언할 수 있음

- **<%-- --%>** : JSP 주석처리

## 최초로 만들어진 JSP의 선언자 구성

최초로 JSP 파일을 만들면 다음과 같은 구성으로 되어있다.

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="EUC-KR" />
    <title>Insert title here</title>
  </head>
  <body></body>
</html>
```

여기서 HTML 코드가 시작하기 전, 지시자를 살펴보면,

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
pageEncoding="EUC-KR"%>
```

사용 언어는 자바, contentType은 Servlet의 `response.setContentType()`과 같은 기능, pageEncoding은 해당 페이지의 인코딩 부분이라는 의미

## JSP 생명주기

- JSP는 Servlet으로 변환되기 때문에, Servlet과 생명주기가 동일하다.(Servlet의 생명주기는 init(), service(), destroy()로 구성된다.)

- Scriptlet(<% ..%>) 안에서의 자바 코드는 모두 Servelt의 `service()` 메소드 안에 포함되있는 자바 코드로 변환된다.

- JSP가 Servlet으로 변환될 때 `init()`, `destroy()` 안에 실행할 수 있는 자바 코드를 만들어주고 싶을 땐, `service()` 메서드와 별개로 필드와 메서드를 선언할 수 있는 선언문(<%! %>)을 이용해 주어야 한다.

- `init()`은 선언문에서 `_jspInit()`로, `destroy()`는 선언문에서 `_jspDestroy()`로 선언할 수 있다.
