---
title: "[Spring] 스프링 시큐리티 Authentication"
tags:
  - Spring
date: 2020-09-03T08:06:00-05:00
key: "[Spring] 스프링 시큐리티 Authentication"
---

# Authentication

스프링 시큐리티의 로그인 과정에선 Authentication의 생성이 필요하다.

<!--more-->

## 전체 과정

![1](/assets/images/200903-1.png)

스프링 시큐리티를 이용한 로그인의 전체적인 과정이다.`(출처 - https://springbootdev.com/)`

### Http Request -> 요청

우선 Http Request의 Method 중 post 방식으로 로그인 요청을 날린다.<br>이때 로그인하고자 하는 사용자의 아이디(User name)와 비밀번호가 Post 방식으로 전달된다.<br>

### AuthenticationFilter

AuthenticationFilter가 Post 방식으로 날라온 요청을 받고 이 필터가 아이디와 패스워드를 가지고 토큰을 만든다.<br>
이때 토큰을 `UsernamePasswordAuthenticationToken`이라고 하며, 이 토큰을 `AuthenticationManager`로 보내준다.<br>

### AuthenticationManager
`UsernamePasswordAuthenticationToken`을 가지고 `AuthenticationManager`가 Authentication 객체를 만들기 위해 토큰을 `UserDetailsService`로 보내준다.<br>

#### UserDetailsService
`UserDetailsService`가 해당 아이디가 DB에 있는지의 여부를 확인해 준다.<br>

이때, 패스워드는 인코딩 과정을 거쳐야 하므로 스프링이 따로 관리해야 하기 때문에 `AuthenticationManager`가 해준다.<br>

### Authentication 객체
이렇게 토큰의 두 값을 확인한 뒤 일치하면 세션에 `Authentication`으로 저장을 해준다.<br>
`Authentication` 객체가 들어간 순간부터 세션 속 시큐리티 컨텍스트에 값이 들어간 상태이므로 필요한 곳에서 사용할 수 있다.<br>