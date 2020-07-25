---
layout: article
title: "[JSP/Servlet] 상태 정보 Scope"
tags:
  - JSP/Servlet
date: 2020-07-25T08:06:00-05:00
key: "[JSP/Servlet] 상태 정보 Scope"
---

# 상태 정보 Scope

<!--more-->

Servelt과 JSP는 네 가지의 범위(**Application > Session > Request > Page**)에서 상태 정보를 저장하고 사용한다.<br>

- 구성 <br>![](/assets/images/200725-1.png)

## 공통 특성

- setAttribue("자료 이름", 자료)로 변수를 저장하고, getAttribute(자료 이름)으로 변수를 불러온다.

## Page Scope

- 한 페이지 내에서 사용할 수 있다.
- forwarding을 이용해서 다른 페이지로 변수를 전달할 수 없다.
- 지역변수처럼 사용할 수 있다.
- JSP에서는 `pageContext` 내장 객체를 이용하여 사용할 수 있다.

## Request Scope

- 클라이언트가 보낸 요청에 대한 응답까지 유지된다.
- Servlet에선 HttpServletRequest 객체를 이용하여 사용한다.
- JSP에선 request 내장 객체를 이용해서 사용한다.
- Request Scope 상태 정보를 유지한 채로 다른 페이지로 전달할 때 forward를 이용한다.

## Session Scope

- 클라이언트마다 상태 정보를 저장할 때 사용한다.
- Servlet에서는 HttpSession 객체를 이용하여 사용한다.
- JSP에서는 session 내장 객체를 이용해서 사용한다.

## Application Scope

- 하나의 웹 어플리케이션이 시작되고 종료될 때까지 유지된다.
- Servlet에서는 ServletContext 자료형의 getServletContext()를 이용하여 사용한다.
- JSP에서는 application 내장 객체를 이용한다.
- 모든 클라이언트들이 값을 공유한다.
