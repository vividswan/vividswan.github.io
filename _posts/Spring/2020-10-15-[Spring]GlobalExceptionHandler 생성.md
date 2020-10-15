---
title: "[Spring] GlobalExceptionHandler 생성"
tags:
  - Spring
date: 2020-10-15T08:06:00-05:00
key: "[Spring] GlobalExceptionHandler 생성"
---

# GlobalExceptionHandler 생성

서버에서 발생하는 오류가 발생했을 때 이를 필터링해주고 오류 페이지로 처리해 주는 Global Exception Handler를 생성하자.<br>

<!--more-->

## Exception

### TaskService

```java
    @Transactional(readOnly = true)
    public Task findTask(long id) {
        Task requestTask = taskRepository.findById(id).orElseThrow(()->{
            return new IllegalArgumentException("해당 task를 찾을 수 없습니다.");
        });
        return requestTask;
    }
```

다음과 같이 `Task`라는 Entity를 조회하는 Service 객체가 있다고 하자.<br>
id를 통해 데이터를 찾으면 return 해주지만, 해당하는 id가 없을 땐 `IllegalArgumentException` 예외를 return 한다.<br>

### 예외 발생

Task 테이블에 존재하지 않는 id를 조회하여 Exception을 호출해보자.<br>
![1](/assets/images/201015-1.png)<br>
`Whitelabel Error Page` 페이지가 생성되며, 오류에 대한 로그가 출력된다.<br>
이러한 로그들을 클라이언트에게 모두 보여주는 건 좋지 않은 웹페이지이므로, 오류를 필터링해주는 컨트롤러를 만들고, 오류에 대한 페이지를 만들어서 따로 보여줘야 한다.<br>

## GlobalExceptionHandler

`handler`라는 패키지를 만들고 `GlobalExceptionHandler`라는 클래스를 생성해 주고 다음과 같이 작성하자.<br>

```java
package com.vividswan.studymate.handler;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestController;

@RestController
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(value = Exception.class)
    public String handleException(Exception e){
        return e.getMessage();
    }
}
```
우선 error의 `getMessage()` 메서드가 잘 출력되는지 확인하기 위해 `RestController`로 생성해 주자.<br>
`@ControllerAdvice`는 Controller에서 발생한 예외 처리를 한곳에서 처리할 수 있도록 지원해 주는 어노테이션이다.<br>
`@handleException`를 메서드에 선언해 준다.<br>
이때 value는 `Exception.class`로 선언하였다.<br>
위의 예제의 `IllegalArgumentException.class`로 선언한다면, 인자에 대한 예외 처리를 해줄 수 있지만, Exception.class는 모든 예외 클래스의 부모이므로 다른 예외들도 처리할 수 있다.<br>
메서드의 인자를 `Exception e`로 받고 e.getMessage()를 return 해준다.<br>

위의 오류를 다시 발생시켜보자.<br>
![2](/assets/images/201015-2.png)<br>
`e.getMessage()`만 출력되었다.<br>

## Exception View

Exception에 대한 view 페이지를 Mustache로 간단하게 만들어보자.<br>
우선 위에서 만든 RestController를 Controller로 바꿔주어 페이지를 호출할 수 있도록 하자.<br>

```java
package com.vividswan.studymate.handler;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@Controller
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(value = Exception.class)
    public String handleException(Exception e,Model model){
        model.addAttribute("errorMessage",e.getMessage());
        return "errorView";
    }
}
```
이제 errorView 페이지를 간단하게 작성하자.<br>
mustache와 제이쿼리를 사용해서 작성한다.<br>

### errorView

```html
<script>
    const backFunc = function (){
        history.back();
    }
</script>
<br>
<div class="container">
    <h1>Error Message Page</h1>
    <hr>
    <div class="d-flex justify-content-center">
    <h3>{{errorMessage}}</h3>
    </div>
    <hr>
    <div class="d-flex flex-row-reverse">
        <button onclick="backFunc()" class="btn btn-primary">뒤로 가기</button>
    </div>
</div>
```
html body 부분에 다음과 같이 간단한 페이지를 작성했다.<br>
`onClick`을 통해 뒤로 가기 버튼도 생성하여 돌아갈 수 있게 작성했다.<br>

![3](/assets/images/201015-3.png)<br>

이로써 `GlobalExceptionHandler`을 통한 에러 페이지를 완성했다.<br>