---
layout: article
title: "[JSP+Servlet] Servlet 생명주기"
tags:
  - JSP/Servlet
date: 2020-07-16T08:06:00-05:00
key: "[JSP+Servlet] Servlet 생명주기"
---

# Servlet 생명주기

Servlet의 생명주기를 이해하기 위해 init(), service(), destroy() 메소드를 알아야 한다.

<!--more-->

## init()

- WAS가 서블릿 요청을 받았을 때, 해당 서블릿이 메모리에 없다면, 그 서블릿을 메모리에 올린다.
- 이때 최초 한 번 init() 메소드가 실행된다.

## service()

- 해당 서블릿이 요청을 받을 때마다 service() 메소드가 실행된다.

## destroy()

- 서블릿의 내용이 갱신되서 웹 어플리케이션이 갱신되거나, WAS가 종료될 때 destroy() 메소드가 실행된다.

---

생명 주기에 관한 메서드를 오버라이드 하는 예제를 통해 확인해보자.

```java


import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/LifeCycleExample")
public class LifeCycleExample extends HttpServlet {
	private static final long serialVersionUID = 1L;
    public LifeCycleExample() {
        super();
        // TODO Auto-generated constructor stub
    }

    @Override
    public void init() throws ServletException {
    	System.out.println("init() 메소드");
    }

    @Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("service() 메소드");
	}

    @Override
    public void destroy() {
    	System.out.println("destroy() 메소드");
    }

}
```

위의 소스를 서버로 실행한 뒤, 콘솔 창을 확인해 보면 메소드가 실행됬음을 알 수 있다.<br><br>
`init() 메소드`<br>
`service() 메소드`<br>
<br>
destroy()의 실행 시점을 확인하기위해, <br>
`System.out.println("destroy() 메소드")`를 System.`out.println("destroy() 메소드!!")` 와 같이 변경하여 저장하면, 서블릿의 내용이 갱신되고, 콘솔 창을 확인해보면<br><br>

`destroy() 메소드`<br><br>
와 같은 로그가 나옴을 알 수 있다.<br>

## doGet(), doPost()

service()는 클라이언트의 요청 방식에 상관없이 서블릿을 요청할 때마다 작동하지만, doGet()은 get 방식으로의 요청에만, doPost()는 post 방식으로의 요청에만 동작한다.<br>
doGet(), doPost()는 모두 service() 안에 내장되어 있고, 오버라이드 한 상태에서, 맞는 방식으로 클라이언트가 요청하면 서블릿의 service()에서 doGet()과 doPost() 중 맞는 메서드를 실행시켜준다.<br>
만약, doGet()에 클라이언트에게 입력받은 정보를 post 방식으로 같은 주소로 전달해 주는 것을 구현하고 doPost()에 그 정보를 출력하는 것을 구현해 준다면,<br>
클라이언트가 처음에 주소창에 get 방식으로 서블릿과 매핑된 주소로 접속하면 doGet()의 페이지가, 페이지에서 정보를 입력하여 전달하면 같은 주소 안에서 doGet()의 페이지가 출력된다.
