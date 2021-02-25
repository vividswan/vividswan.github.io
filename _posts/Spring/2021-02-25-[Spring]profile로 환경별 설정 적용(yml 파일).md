---
title: "[Spring] profile로 환경별 설정 적용(yml 파일)"
tags:
  - Spring
date: 2021-02-25T08:06:00-05:00
key: "[Spring] profile로 환경별 설정 적용(yml 파일)"
---

# profile로 환경별 설정 적용

yml 파일에 profile로 local, dev 환경별 설정을 따로 적용해보자.

<!--more-->

## applicatio.yml

```yml
#...
server:
  port: 5000
  servlet:
    context-path: /
    encoding:
      charset: UTF-8
      enabled: true
      force: true

spring:
  profiles: # profiles 설정
    active: local # 다른 설정이 없을 때 default 환경 값
  devtools:
    livereload:
      enabled: true
  jpa:
    hibernate:
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
    show-sql: true
#...
# application.yml엔 공통으로 적용할 설정을 적는다.
```

spirng.profiles.active에는 따로 지정해둔 설정이 없을 때 default 값을 적어둔다.<br>
위의 소스에선 다른 설정이 없을 시 local로 프로젝트가 빌드 된다.<br>

## application-dev.yml, applicatio-local.yml

![1](/assets/images/210225-1.png)<br>

프로젝트의 src/main/resources의 하단에 있는 application.yml과 동일한 위치에 설정하고 싶은 환경 파일을 다음과 같이 만들어준다.<br>
`application-{환경명}.yml`<br>

### application-dev.yml

```yml
spring:
  jpa:
    hibernate:
      ddl-auto: update #create update none
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: {db url}
    username: {db username}
    password: {db password}
  mail:
    host: smtp.gmail.com
    port: 587
    username: {smtp mail service username}
    password: {smtp mail service password}
    properties:
      mail:
        smtp:
          auth: true
          starttls:
            enable: true
```

local과 다르게 할 설정들을 `application-dev.yml`에 세팅한다.<br>
ddl-auto 방식이나 local 과는 다른 db 설정, 그리고 배포 시에만 사용할 mail 기능을 추가하였다.<br>

### application-local.yml

```yml
spring:
  jpa:
    hibernate:
      ddl-auto: create #create update none
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: {db url}
    username: {db username}
    password: {db password}
```

local 같은 경우엔 배포할 때와 다른 db를 사용하고 ddl-auto 방식도 create, mail 기능을 안 쓰기 때문에 다음과 같이 생성하였다.<br>

### ConsoleMailSender.java

```java
@Profile("local")
@Slf4j
@Component
public class ConsoleMailSender implements JavaMailSender {

 // ...

}
```
로컬에서는 직접 메일을 전송하지 않으므로 위와 같이 `JavaMailSender`을 implements 한 클래스로 로깅 처리를 해주었다.<br>
이 때 해당 클래스는 local에서만 쓰이므로 `@Profile("local")` 어노테이션을 추가해주었다.<br>

## 빌드

### IntelliJ
![3](/assets/images/210225-3.png)<br>
IntelliJ IDE를 이용할 시엔 다음과 같이 우측상단에서 `Edit Configurations`를 클릭하면 다음과 같은 창이 나온다.<br>

![2](/assets/images/210225-2.png)<br>

중간에 있는 Active Profiles에 원하는 환경을 입력하면 된다.<br>
공란으로 둘 시에는 application.yml에 있는 default 값으로 설정된다.<br>

### 서버로 배포 시

서버에서는 `-Dspring.profiles.active` 옵션을 적용하면 된다.<br>

> $ java -jar -Dspring.profiles.active={지정할 환경} api-0.0.1-SNAPSHOT.jar<br>

위에 `지정할 환경`에 원하는 환경을 입력하면 해당 환경으로 배포된다.<br>