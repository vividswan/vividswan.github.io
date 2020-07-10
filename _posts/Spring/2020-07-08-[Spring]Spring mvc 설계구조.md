---
title: "[Spring] Spring mvc 설계구조"
tags:
  - Spring
date: 2020-07-08T08:06:00-05:00
key: "[Spring] Spring mvc 설계구조"
---

## Spring mvc

![](/assets/images/200708-1.png){: .align-center}
`Spring mvc 설계`<br>

### jsp & servlet에서 확장된 구조

스프링에 대한 설계구조를 다음과 같이 나타낼 수 있다.<br>
기존 jsp&servlet과 다르게 DispatcherServlet, HandlerMapping, HandlerAdapter, ViewResolver가 추가되었다.<br>

### DispatcherServlet

클라이언트에서 요청을 하면 DispatcherServlet로 요청이 전달되며, HandlerMapping, HandlerAdapter, ViewResolver 순으로 요청을 주고받는다.<br>

### HandlerMapping(핸들러매핑)

클라이언트의 요청을 처리할 수 있는 컨트롤러를 찾아준다.<br>
java파일에서 @Controller 어노테이션을 붙여주면 핸들러매핑이 찾아 줄 수 있다.<br>

### HandlerAdapter(핸들러어뎁터)

핸들러매핑에서 찾은 컨트롤러 안에서 요청을 처리할 수 있는 가장 적합한 메서드를 찾아준다.<br>
java파일에서 @RequestMapping(value=`"url 경로"`, method= `get/post 방식`)의 어노테이션으로 설정해준다.<br>

### ViewResolver

컨트롤러가 return한 jsp페이지를 `해당경로/return값.jsp`로 변환해주어 요청에 맞는 처리결과를 출력할 view 페이지를 선택해준다.<br>

### example

DB 자료를 읽어내는 가상의 viewDB를 예를들어 만들어보자.<br>

```java
@Controller // 핸들러매핑이 찾을 수 있도록 컨트롤러임을 알려주는 어노테이션 사용
public class viewDB{

    @RequestMapping(value="/view", method= RequestMethod.GET)
    // 핸들러어뎁터가 찾아줄 수 있도록 메서드임을 알려주는 어노테이션 사용(url경로는 /view이고 GET방식(생략가능)임)
    public String view(Model model){ // 모델값을 인자로 받을 수 있다.
        model.setAttribute("view","viewValue"); // 응답으로 전달되는 데이터(모델)

        return "view"; // ViewResolver에 의해 view.jsp로 변환
    }
}
```
