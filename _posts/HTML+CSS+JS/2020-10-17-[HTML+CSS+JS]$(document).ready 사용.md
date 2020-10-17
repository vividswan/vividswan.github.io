---
layout: article
title: "[HTML/CSS/JS] $(document).ready 사용"
tags:
  - HTML/CSS/JS
date: 2020-10-17T08:06:00-05:00
key: "[HTML/CSS/JS] $(document).ready 사용"
---
# $(document).ready 사용

제이쿼리에서 $(document).ready을 사용해서 로그인 요청 알람을 만들어보자.<br>

<!--more-->

## 로그인 페이지

로그인이 안 된 클라이언트가 로그인이 필요한 페이지에 들어가려고 하면 어떻게 해야할까?<br>
서버 입장에서는 자동으로 로그인을 할 수 있는 페이지로 이동시켜줘서 클라이언트에게 로그인을 할 수 있게 도와줘야한다.<br>
그런데 클라이언트에게 아무런 메세지 없이 바로 로그인 페이지로 이동하면, 클라이언트 입장에선 오류로 인식할 수도 있고 원하는 페이지가 로그인을 요구하는 페이지인건지 알 수 없다.<br>
그러므로 로그인이 필요하다는 alert를 이용해 클라이언트가 이를 이해하고 로그인 할 수 있게 도와주자.<br>

## $(document).ready
로그인이 필요한 페이지로 이동하자마자 바로 alert가 나타나게 하려면 어떻게 해야할까?<br>
제이쿼리에선 이를 `$(document).ready`를 제공해 구현할 수 있게 해준다.<br>

```javascript
   $(document).ready("구현하고자 하는 함수");
```
`$(document).ready`에 파라미터로 들어간 함수는 클라이언트가 해당 페이지로 이동 했을 때 바로 실행된다.<br>
이를 이용해서 구현해보자.<br>

## 스프링 부트 컨트롤러

```java
    @GetMapping("loginForm/{first}")
    public String loginForm(Model model, @PathVariable int first)
    {
        boolean chk;
        if(first==0){
            chk =false;
        }
        else chk = true;
        model.addAttribute("chk",chk);
        return "user/loginForm";
    }
```

스프링 부트 컨트롤러에서 로그인 페이지를 get 요청을 했을 때, 해당 하는 페이지를 return 하게 해주었다.<br>
여기서 uri의 `first`는 클라이언트가 직접 로그인 페이지로 접속했을 때와, 스프링 부트가 로그인 페이지로 자동으로 연결해 줬을 때의 상태를 구별하는 변수로 사용했다.<br>

## mustache + jquery

```javascript
{{^chk}}
  <script>
     $(document).ready(function(){
          alert("로그인이 필요합니다.");
      });
  </script>
  {{/chk}}
```

프론트는 mustache를 이용해 구현했고, jquery에 관련된 라이브러리를 import하여 사용하였다.<br>
앞서 나왔던 컨트롤러서에서 `chk`라는 변수를 false로 주면 로그인 alert가 나오게 구현하였다.<br>
`chk==true`라면, 클라이언트가 해당 페이지에 접속했을 때 `$(document).ready`에 의해 `function(){alert("로그인이 필요합니다.");}` 함수가 자동으로 실행된다.<br>

## 확인

![1](/assets/images/201017-1.png)<br>

로그인이 필요한 페이지를 접속하면 바로 로그인 요청 alert를 확인할 수 있다.<br>