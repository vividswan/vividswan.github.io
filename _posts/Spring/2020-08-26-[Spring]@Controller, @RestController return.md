---
title: "[Spring] @Controller, @RestController return"
tags:
  - Spring
date: 2020-08-26T08:06:00-05:00
key: "[Spring] @Controller, @RestController return"
---

# @Controller, @RestController return

@Controller와 @RestController 어노테이션이 붙은 컨트롤러의 return을 알아보자.

<!--more-->

## @Controller

Rest가 없는 Controller로 선언하고 String 값을 리턴해보자.<br>

```java
package com.vividswan.blog.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class UserController {

	@GetMapping("/user/loginForm")
	public String loginForm() {
		return "user/loginForm";
	}

}
```

클라이언트에게 `"/user/loginForm"`의 경로로 get 요청을 받았고, 로그인 페이지를 전달해 줘야 한다고 생각해보자.<br>
get 요청에 대한 응답을 하기 위해 컨트롤러를 만들어야 되고, 컨트롤러를 거친 뒤 해당 주소에 맞는 로그인 페이지를 reponse 해야 한다.<br>

이때 `@Controller` 어노테이션으로 컨트롤러를 만들고, 매핑된 메서드로 String 값을 리턴하면, view resolver가 return에 관여한다.<br>

### View Resolver

Controller에서 return 값으로 String 자료형을 return 하면 View Resolver가 String과 매칭되는 화면을 찾아서 클라이언트에게 전송해 준다.<br><br>

설정이 안 들어간 기본적인 View Resolver의 매핑은 `templates/` + `반환 값` + `.html` 형식이다.<br>

즉, "hello"라는 값을 return 한다면, templates 폴더 아래에 `hello.html`을 반환해 준다.<br>

#### View Resolver 경로 변경

스프링 부트의 application.yaml 등의 설정 파일에서 변경할 수 있다.<br>
jsp로 응답하고 싶다면 아래와 같이 변경할 수도 있다.<br>

```yaml
mvc:
  view:
    prefix: /WEB-INF/views/
    suffix: .jsp
```

위와 같이 설정한 뒤, 정확한 위치에 return값.jsp 파일이 있다면, View Resolver가 해당 파일을 화면에 띄워줄 것이다.

## @RestController

RestController는 단순한 String 값을 보낼 수도 있고, 객체를 보낼 수도 있다.<br>

### String return

```java
package com.vividswan.blog.test;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class BlogControllerTest {

	@GetMapping("/test/hello")
	public String hello() {
		return "<h1>hello spring boot</h1>";
	}
}
```

위와 같이 단순히 `"<h1>hello spring boot</h1>"`를 return 하는 컨트롤러를 만들고 매핑된 주소에 get 요청을 해보자.<br>

![1](/assets/images/200826-1.png)

올바른 경로라면 String 값을 return 하는 것을 확인할 수 있다.<br>

### 객체 return

```java
package com.vividswan.blog.test;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

// 스프링이 com.vividswan.blog 패키지 이하를 스캔해서 모든 파일을 메모리에 new 하는 것은 아니다.
// 특정 어노테이션이 붙어있는 클래스 파일들을 new해서(IoC) 스프링 컨테이너에 관리해준다.

@RestController
public class BlogControllerTest {

	//http://localhost:8080/test/hello
	@GetMapping("/test/hello")
	public Member hello() {
		Member member = new Member();
		member.setUsername("vividswan");
		member.setEmail("vividswan@naver.com");
		member.setPassword("12345");

		return member;
	}
}
```

위 컨트롤러의 hello 메서드를 살짝 바꿔서, 멤버라는 객체를 return 해보자.<br>

### JSON으로 변환해 주는 MessageConverter

![2](/assets/images/200826-2.png)

**java object 형식의 객체가 아니라 JSON으로 바뀌어서 전송되었다!**

@RestController의 어노테이션인 컨트롤러의 메서드에서 객체 값을 리턴한다면, MessageConverter의 jackson이 java object를 JSON 형태로 바꿔서 리턴해준다.<br>

이러한 방식으로 프론트단으로 자료를 넘겨줄 때, JSON 형식의 자료를 보내줄 수 있으므로, 프론트단도 JSON 형식의 자료를 가공하여 그에 맞게 개발이 가능하다.<br>
