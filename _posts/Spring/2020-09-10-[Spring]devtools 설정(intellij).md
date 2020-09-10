---
title: "[Spring] devtools 설정(intellij)"
tags:
  - Spring
date: 2020-09-10T08:06:00-05:00
key: "[Spring] devtools 설정(intellij)"
---

# devtools 설정

변경사항을 자동으로 빌드 해주는 devtools를 gradle 프로젝트에서 사용하기 위한 설정을 알아보자.

<!--more-->

## build.gradle 의존관계 설정

```java
dependencies {
  //...
  developmentOnly 'org.springframework.boot:spring-boot-devtools'
  //...
}
```
build.gradle에 다음과 같은 의존관계를 설정한다.

## setting -> Build, Execution, Deployment -> Compiler

![1](/assets/images/200910-1.png)

해당 부분에 들어가서 Build project automatically를 체크해 준다.<br>

## Registry 설정

`ctrl + shift + alt + /` 후 registry로 들어가서 다음과 같은 항목을 체크한다.<br>

![2](/assets/images/200910-2.png)
`compiler.automake.allow.when.app.running` 항목이다.<br>

## application.yml 설정

`application.yml`에 다음과 같은 설정을 추가한다.<br>

```java
spring:
  devtools:
    livereload:
      enabled: true
  freemarker:
    cache: false
```

`application.property`를 사용하면 문법에 맞게 고쳐서 추가한다.<br>

## intellij 재시작

재시작 후 devtool이 작동함을 알 수 있다.