---
title: "[Database] User 생성, 권한 설정"
tags:
  - Database
date: 2020-09-28T08:06:00-05:00
key: "[Database] User 생성, 권한 설정"
---

# MySQL - User 생성, 권한 설정

mysql에서 root 외의 User를 만들고 권한을 설정해보자.
<!--more-->

## User, 데이터베이스 생성

```sql
CREATE USER 'vividswan'@'%' IDENTIFIED BY '12345';
GRNAT ALL PRIVILEGES ON *.* TO 'vividswan'@'%';
CREATE DATABASE TEST;
USE TEST;
```

> CREATE USER 'vividswan'@'%' IDENTIFIED BY '12345';<br>

- `CREATE USER` : user를 만드는 명령어이다.<br>
- `'vividswan'@'%'` : `vividswan`이라는 아이디를 만든다.<br>
(이때, `@` 뒤에 접속할 수 있는 IP의 주소를 적어줘야한다. `@localhost`라고 한다면 로컬에서만 접속할 수 있고, `'%'`로 해준다면 외부에서의 접속을 허용한다는 설정이 된다)<br>

> GRNAT ALL PRIVILEGES ON * . * TO 'vividswan'@'%';

- GRNAT ALL PRIVILEGES : 접근할 수 있는 권한을 주는 명령어이다.<br>
- 스키마명.테이블명 : 접근할 수 있는 스키마명과 테이블명을 지시할 수 있다.<br> 위와 같이 입력하면 모든 스키마와 테이블에 접근할 수 있다.<br>
- TO 'vividswan'@'%' : 권한을 줄 User 정보를 입력한다.<br>

> CREATE DATABASE TEST;

- TEST라는 데이터베이스를 생성한다.

> USE TEST;

- 이제 위 계정으로 TEST 데이터베이스를 사용할 수 있다.<br>

## 접속

기존의 root 접속 명령어 대신 위에서 만든 계정으로 접속할 수 있다.<br>

> MySQL -u vividswan -p

입력 후 비밀번호를 입력해준다.<br>

접속이 완료되면 원하는 데이터베이스를 이용할 수 있다.<br>

## 적용

![1](/assets/images/200928-1.png)<br>