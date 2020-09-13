---
layout: article
title: "[JPA] Optional 처리"
tags:
  - JPA
date: 2020-09-14T08:06:00-05:00
key: "[JPA] Optional 처리"
---

# Optional 처리

JPA의 Repository에서 조회할 때 나오는 Optional 타입을 처리해보자.

<!--more-->

## Service

웹 애플리케이션 계층 구조 중 서비스 계층은 도메인에 대한 정보를 리포지터리에서 조회하고, 정보를 가공하여 컨트롤러로 보내주는 일을 한다.<br>
이때, JPA의 리포지터리에서 find를 하면, 찾고자 하는 객체를 return 하는 것이 아닌 Optional 타입의 객체를 리턴한다.<br>

## Optinal

Optional 타입의 객체를 리턴하는 이유는 서비스에서 조회하고자 하는 도메인에 관한 정보가 없을 때, 단순히 `null`을 리턴하는 게 아니라, 서비스단에서 상황에 맞는 프로그래밍을 할 수 있도록 제공해 주기 위함이다.<br>

Optional 타입으로 리턴할 때 메서드 체인으로 처리할 수 있는 메서드는 다음과 같다.<br>

- **get() :** 도메인의 자료값을 리턴해 줄 때 자료가 존재하지 않으면 `NoSuchElementException` Error 예외를 던져준다.
- **orElseGet() :** 도메인의 자료값을 리턴해 줄 때 자료가 존재하지 않으면 람다식의 결과로 값을 대체한다.
- **orElseThrow() :** 도메인의 자료값을 리턴해 줄 때 자료가 존재하지 않으면 람다식의 결과로 Error 예외를 던져준다.

## orElseThrow()

orElseThrow()를 이용해서 User 도메인을 `findById()`로 찾을 때 Optional 자료형을 처리해보자.<br>

### 제네릭, 오버라이딩

```java
		User user = userRepository.findById(id).orElseThrow(new Supplier<IllegalArgumentException>() {
			@Override
			public IllegalArgumentException get() {
				// TODO Auto-generated method stub
				return new IllegalArgumentException("해당 ID의 user가 존재하지 않습니다. id : "+id);
			}
```
orElseThrow의 제네릭이 Supplier이고, Supplier를 통해 `IllegalArgumentException`를 날려줄 것이므로, IllegalArgumentException을 Supplier의 제네릭으로 사용한다.<br>
그 뒤 Supplier의 get() 메서드를 오버라이딩하여 Error를 리턴해준다.<br>

### 람다식 
```java
User user = userRepository.findById(id).orElseThrow(() -> {
		 		return new IllegalArgumentException("해당 ID의 user가 존재하지 않습니다. id : "+id);
		});
```
`orElseThrow`의 인자로 반드시 Supplier가 올 것을 알고, Supplier 내부에서도 get() 메서드를 오버라이딩 할 것이기 때문에 다음과 같이 람다식을 응용할 수도 있다.
