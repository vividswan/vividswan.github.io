---
layout: article
title: "[JSP/Servlet] Request, Response 객체"
tags:
  - JSP/Servlet
date: 2020-07-17T08:06:00-05:00
key: "[JSP/Servlet] Request, Response 객체"
---

# Request, Response

- 서블릿은 클라이언트의 요청 정보를 Request 객체에 담고, 요청에 대한 응답 정보를 Reponse 객체에 담는다.

<!--more-->

### HttpServletRequest

- HTTP 프로토콜의 Request 정보를 서블릿에게 전달하기 위해 WAS가 생성

- 요청하기 위해 가지고 있는 모든 정보를 모두 메서드로 담음

- 헤더 정보, 파라미터, 쿠키, URI, URL .. 등을 읽기 위한 메서드를 가지고 있음

### HttpServletResponse

- 응답을 보내기 위해 WAS가 HttpServletResponse 객체를 생성하여 서블릿에게 전달

- 응답 코드, 응답 메시지 등을 전송

### 객체 생성 및 사용 과정

- 웹브라우저에서 URL을 입력한다.

- 클라이언트는 도메인과 포트 번호로 서버에 접속된다.
- 클라이언트의 IP, path 정보 등 여러 가지 정보들이 서버에 전송된다.

- 클라이언트로부터 요청이 들어오면 WAS는 HttpServletRequest, HttpServletResponse 생성

- **HttpServletRequest** -> 요청 시 가져온 정보를 객체에 담음

- **HttpServletResponse** -> 클라이언트에게 전송하기 위한 정보를 객체에 담음

- 요청 정보에 있는 path로 매핑된 서블릿에게 객체들이 전달

- doPost(),service() 등의 메서드에 파라미터로 전달되어 사용

- 그림<br> ![](/assets/images/200717-1.png)
