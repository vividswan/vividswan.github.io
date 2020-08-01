---
title: "[Spring] start.spring.io로 프로젝트 생성"
tags:
  - Spring
date: 2020-07-31T08:06:00-05:00
key: "[Spring] start.spring.io로 프로젝트 생성"
---

## start.spring.io

start.spring.io로 손쉽게 스프링 프로젝트를 만들어본다.<br>

<!--more-->

## 접속

주소창에 `https://start.spring.io/`를 입력한 뒤 접속한다.<br>
아래와 같은 화면을 볼 수 있다.<br>
![1](/assets/images/200731-1.png)<br>
하나하나 체크해서 설정을 할 수 있다.<br>

## 요소

![2](/assets/images/200731-2.png)<br>
프로젝트가 Maven 인지, Gradle 인지 설정한다.<br>
프로그램 언어를 설정한다.<br>
스프링 부트의 버전도 설정할 수 있다.<br>
`SNAPSHOT`은 안정화되지 않은 버전, `M` 은 정해진 주기마다 배포되는 버전, 아무것도 안 쓰여있으면 정식 버전이다.<br><br>

![3](/assets/images/200731-3.png)<br>
프로젝트에 대한 정보를 입력하고, Jar, War의 패키징, JDK 버전을 설정한다.<br>

![4](/assets/images/200731-4.png)<br>
의존성을 추가할 수 있다.<br>
`Spring Web`과 `Thymeleaf`를 추가하였다.<br>

하단의 GENERATE를 누른 뒤 생성된 파일을 import 하여 열어주면 생성된다.<br>
intellij의 경우엔 초기 화면의 Open or Import로 만들 수 있다.<br>

## gradle

생성된 프로젝트의 `build.gradle` 파일을 열어보면 설정했던 프로젝트의 정보와 의존성을 확인할 수 있다.<br>

```java
plugins {
	id 'org.springframework.boot' version '2.3.2.RELEASE'
	id 'io.spring.dependency-management' version '1.0.9.RELEASE'
	id 'java'
}

group = 'com.vividswan.simple'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

test {
	useJUnitPlatform()
}

```
