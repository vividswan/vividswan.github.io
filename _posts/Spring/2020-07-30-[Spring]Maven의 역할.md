---
title: "[Spring] Maven의 역할"
tags:
  - Spring
date: 2020-07-30T08:06:00-05:00
key: "[Spring] Maven의 역할"
---

## Maven

<!--more-->

Java 기반의 웹 프로그램을 만들 때, 설치되어 있지 않은 라이브러리를 사용하려면 WEB-INF/lib 폴더에 복사 후 설치하여야 한다.<br>
프로젝트가 커질수록 이런 방식은 복잡함을 유발하므로 이런 문제와 더 나은 컴파일과 배포를 위해 Maven이 등장했다.<br>

## CoC & 디렉토리 구조

CoC는 소스 파일의 위치, 컴파일된 파일의 위치 등의 정해진 관습이다.<br>
Maven은 CoC를 지키며 작동하기 때문에, 이러한 관습들을 이해하여야 한다.<br>
다수가 개발하는 프로젝트에서, 관습에 맞춰서 파일을 저장하고 컴파일하여야 한다.<br>
<br>

- 디렉토리 구조<br>![](/assets/images/200730-1.png)
- java와 관련된 패키지와 소스코드는 **src/main/java**에 위치
- properties와 xml 등과 같은 설정 파일은 **src/main/resources**에 위치
- WEB-INF와 같은 웹 관련 리소스들은 **src/main/webapp**에 위치
- 테스트와 관련된 java 패키지와 소스코드는 **src/test/java**에 위치
- 테스트와 관련된 설정 파일들은 **src/test/resources**에 위치
- 컴파일과 패키징 된 결과물들은 **target**에 위치
- Maven 설정 파일은 **pom.xml**

## pom.xml 기본구성

```xml
<!-- project -> 최상위 루트 엘리먼트 -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <!-- POM model의 버전-->
  <modelVersion>4.0.0</modelVersion>

  <!-- 프로젝트 생성 조직의 고유 아이디, 도메인 이름을 거꾸로한다. -->
  <groupId>kr.or.connect</groupId>
  <!-- artifact 고유 ID -->
  <artifactId>mavenweb</artifactId>
  <!-- 프로젝트의 현재 버전 -->
  <version>0.0.1-SNAPSHOT</version>
  <!-- jar, war, ear 등의 패키징 형식 -->
  <packaging>war</packaging>

   <!-- 프로젝트의 이름 -->
  <name>mavenweb Maven Webapp</name>
  <!-- 프로젝트 사이트 주소 -->
  <url>http://www.example.com</url>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>

```
