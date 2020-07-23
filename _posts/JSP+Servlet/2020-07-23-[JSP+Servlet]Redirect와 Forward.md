---
layout: article
title: "[JSP/Servlet] Redirect와 Forward"
tags:
  - JSP/Servlet
date: 2020-07-23T08:06:00-05:00
key: "[JSP/Servlet] Redirect와 Forward"
---

# Redirect와 Forward

<!--more-->

## Redirect

- 서버가 클라이언트에게 특정 URL로 이동을 요청하는 것

- 처음에 클라이언트가 보낸 요청에 대한 응답 코드 302와 헤더 내 Location 값에 URL을 보냄

- **HttpServletResponse**의 **sendRedirect()** 메서드를 사용해서 리다이렉트함

- 클라이언트가 서버의 요청에 의해 새로운 URL을 요청하는 것이므로, **Request Scope의 상태 정보가 유지되지 않는다.**

- 새로운 URL을 요청하는 것이므로, 주소창의 URL이 변한다.

- 같은 웹 어플리케이션이 아니어도 Redirect로 이동할 수 있다.

### Redirct의 구현

```java
  response.sendRedirect("Redirect 할 페이지");
```

Servlet에선, service(), doGet(), doPost() 등에 인자로 들어가 있는 `HttpServletResponse`의 sendRedirct() 메서드를 통해 이동한다.<br>
JSP에선 내장 객체로 HttpServletResponse가 들어있으므로, response.sendRedirct()로 이동한다.<br>

## Forwarding

- 서버가 클라이언트의 요청을 처리하다가, 다른 서블릿이나 JSP에게 추가적인 처리를 하기 위해 요청을 위임하는 것

- Forwarding의 주소는 '/'로 시작한다.

- Forwarding은 같은 웹 어플리케이션에서만 이동 가능하다.

- 클라이언트에게 요청에 대한 응답이 전달되지 않은 상태이므로, Forwarding을 해도 요청과 응답이 유지된다.

- 요청과 응답이 유지되기 때문에 **Request Scope의 상태 정보가 유지된다.**

- Servlet으로 로직을 처리하고, Servlet의 데이터를 JSP로 Forwarding 하는 방식으로 Servlet과 JSP를 연동한다.

- Forwarding 방식은 주소창의 URL이 변하지 않는다.

- 클라이언트 입장에선 요청에 대한 정보만 받으므로, 어떤 서블릿들이 Forwarding으로 요청을 처리했는지 알 수 없다.

### Forwarding의 구현

```java
  String data = "보내고 싶은 자료";
	request.setAttribute("dataName", data);
	RequestDispatcher requestDispatcher = request.getRequestDispatcher("/path");
	requestDispatcher.forward(request, response);
```

보내고 싶은 자료를 `String data`라고 할 때, 우선 request의 `setAttribute`로 원하는 데이터의 이름과 데이터 값을 HttpServletRequest에 저장해 준다. 이때, data는 String형이 아닌 Object형으로 저장된다.<br> RequestDispatcher를 getRequestDispatcher로 호출하는데, 이때 Forwarding을 원하는 주소로 써주어야 하고, '/'로 시작하여아한다.<br>
마지막으로 requestDispatcher의 forward() 메서드를 통해 request와 response 객체가 /path로 전달된다.<br>

```java
  String data = (String)request.getAttribute("dataName");
```

호출 받은 Servlet에선 request, response 객체가 Forwarding을 통해 유지되어 있으므로, request의 `getAttribute()`로 값을 받을 수 있다.<br>
이때의 자료는 Object형이므로, 원하는 자료형으로 형 변환을 해주어야 한다.<br>
