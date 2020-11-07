---
title: "[Spring] maven 프로젝트에서 h2 실행"
tags:
  - Spring
date: 2020-11-07T08:06:00-05:00
key: "[Spring] maven 프로젝트에서 h2 실행"
---

# maven 프로젝트에서 h2 실행

<!--more-->

## pom.xml

```xml
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
```

스프링 부트 이니셜 라이저는 버전에 대한 설정이 없이 위의 형태로 dependency를 생성해 준다.<br>
보안에 대한 이슈가 추가된 1.4.198 이상의 버전은 충돌이 일어날 수 있으므로 `<version>1.4.197</version>`을 추가해서 다음과 같이 dependency를 설정하자.<br>

```xml
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <version>1.4.197</version>
            <scope>runtime</scope>
        </dependency>
```

## application.yml or property

application.yml or property에 다음과 같이 추가해 준다.<br>

```yml
spring.datasource.url: jdbc:h2:mem:testdb
spring.datasource.driverClassName: org.h2.Driver
spring.datasource.username: sa
spring.datasource.password:
spring.jpa.database-platform: org.hibernate.dialect.H2Dialect
```

h2 접속에 대한 경로의 정보와 username, password, platform의 설정이다.<br>

> spring.jpa.hibernate.ddl-auto: create

추가로 다음과 같은 설정으로 서버 시작 시 db 생성 여부를 설정할 수 있다.<br>
`update`로 변경하면 서버가 꺼져도 db의 데이터가 영속적으로 저장된다.<br>

## 접속

스프링 부트 서버를 실행하고 url에 다음과 같이 접속한다.<br>
> http://localhost:8080/h2-console


![1](/assets/images/201107-1.png)
위와 같이 입력한 뒤 `Connect`를 눌러준다.<br>

![2](/assets/images/201107-2.png)
접속이 완료됨을 확인할 수 있다.<br>