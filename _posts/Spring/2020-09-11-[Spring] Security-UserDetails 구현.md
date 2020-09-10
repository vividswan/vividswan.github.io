---
title: "[Spring] Security-UserDetails 구현"
tags:
  - Spring
date: 2020-09-11T08:06:00-05:00
key: "[Spring] Security-UserDetails 구현"
---

# UserDetails 구현

Spring Security 기능에 필요한 UserDetails를 알아보고 구현해보자.

<!--more-->

## Spring Security 로그인 필터링

스프링 시큐리티의 로그인 과정은 `WebSecurityConfigurerAdapter`을 구현한 클래스에서 전반적인 설정이 가능하다.<br>
이때 `http.authorizeRequests()`의 메서드 체인 중 `loginPage()`의 인자로 경로를 넣으면 해당 경로의 로그인 요청을 스프링 시큐리티가 필터링하여 로그인이 진행된다.<br>

## Spring Security Session

스프링은 로그인의 진행이 정상적으로 완료되면 스프링 시큐리티를 위한 session을 만들어주어 로그인 처리를 해준다.<br>
이 session은 웹 서버의 같은 session 공간 속 `Security ContextHolder`라는 키값 안에 저장된다.<br>

## Session의 오브젝트 타입 -> Authentication

이때, 스프링 시큐리티 세션에 들어갈 수 있는 오브젝트 타입은 `Authentication` 타입이다.<br>
그러므로, `Authentication` 타입 안에 로그인을 한 User에 대한 정보가 들어 있어야 하는데, 여기서 중요한 점은 Authentication로 감쌀 수 있는 User의 타입이 그냥 User 오브젝트가 아닌 `UserDetails` 타입만 가능하다.<br>

## UserDetails 구현

이러한 위의 과정 때문에 스프링 시큐리티의 로그인에선 `UserDetails`로 User에 관한 정보를 전달해 준다.<br>
원하는 정보를 담기 위해 UserDetails를 확장한 클래스를 생성하고 오버라이딩하여 구현한다.<br>

### 전체 구현

```java
package com.security.vividswan.auth;

import com.security.vividswan.model.User;
import org.springframework.security.core.GrantedAuthority;

import org.springframework.security.core.userdetails.UserDetails;

import java.util.ArrayList;
import java.util.Collection;

public class PrincipalDetails implements UserDetails {
    private User user; // composition

    PrincipalDetails(User user){
        this.user = user;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        Collection<GrantedAuthority> collect = new ArrayList<>();
        collect.add(new GrantedAuthority() {
            @Override
            public String getAuthority() {
                return user.getRole();
            }
        });
        return collect;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUsername();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

## 코드 설명

```java
    private User user; // composition

    PrincipalDetails(User user){
        this.user = user;
    }
```
원하는 user 정보를 담아주기 위해 User 객체를 컴포지션한다.<br>
이 클래스는 new 생성자를 통해 `Authentication`에서 사용할 것이므로 따로 어노테이션을 해주지 않는다.<br>
이를 위해 `User` 오브젝트가 들어가는 생성자를 만든다.<br>

```java
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        Collection<GrantedAuthority> collect = new ArrayList<>();
        collect.add(new GrantedAuthority() {
            @Override
            public String getAuthority() {
                return user.getRole();
            }
        });
        return collect;
    }
```
User의 Role을 넣어줘야 하는 `getAuthorities()` 메서드다.<br>
한 명의 User의 Role이 복수일 수 있으므로 Collection 형식인데 `<GrantedAuthority>`의 제네릭을 따라줘야 한다.<br>
필요한 제네릭을 따르는 ArrayList를 만들어서 리스트에 오브젝트를 넣어줘야 한다.<br>
이때 `GrantedAuthority` 형식을 지키기 위해 `new GrantedAuthority()`로 새로운 인스턴스를 생성한다.<br>
`getAuthority()`를 오버라이드 해줘야 하는데 이때 User의 Role을 String 형식으로 리턴하면 리스트에 오브젝트 삽입이 가능하다.<br>

```java
    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUsername();
    }
```
get과 관련된 메서드엔 User의 정보를 return 해준다.<br>

```java
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
```
가입된 User에 대한 기간 만료, 잠금 상태 등등을 설정할 수 있는 메서드다.<br>
위의 예제에선 모두 true 처리했지만 필요한 조건들을 만들고 분기적으로 처리하여 사용할 수 있다.<br>