---
layout: article
title: "[Database] MySQL 접속하기 & 데이터베이스 생성"
tags:
  - Database
date: 2020-07-27T08:06:00-05:00
key: "[Database] MySQL 접속하기 & 데이터베이스 생성"
---

# MySQL 접속하기 & 데이터베이스 생성

<!--more-->

MySQL에 접속해서 데이터 베이스를 생성해본다.<br>

## Root로 관리 시스템에 접속

cmd 창을 키고 다음과 같이 입력한다.<br>

```sql
mysql -u root -p
```

그러면 cmd 창에서 `Enter password:` 라는 메시지가 나온다.<br>
MySQL 설치 시에 설정했던 비밀번호를 입력해준다.<br>

![](/assets/images/200727-1.png)

다음과 같은 화면으로 접속을 해줄 수 있다.<br>

## Database 생성

```sql
create database testDB;
```

다음과 같이 입력하면, `Query OK, 1 row affected (0.07 sec)`라는 메시지가 뜬다.<br>
Query는 명령을 내리는 문장인데, Query가 완료되었고, 0.07초의 수행 시간이 걸렸다는 의미이다.<br>

## Database 사용하기 & 확인하기

`use` 명령어를 이용해 사용할 데이터베이스로 전환한다.<br>

```sql
use TestDB;
```

`Database changed`라는 메시지가 나온다.<br><br>

`show database` 명령어를 이용하면 MySQL에 존재하는 데이터 베이스를 확인할 수 있다.<br>

```sql
show databases;
```

## Table

Database는 행과 열로 된 테이블 형태로 자료를 저장한다.<br>
이렇게 데이터들이 모두 테이블 형태로 저장되어 있는 것을 **관계형 DB** 라고 한다.<br>

- 열 : 단일 종류의 데이터를 나타내며, 특정한 데이터 타입과 크기를 가지고 있다.
- 행 : 열들의 값의 조합이며, 기본 키에 의해 구분되며, 이때 기본 키는 중복을 허용하면 안 되고 없어서는 안 된다. **레코드**라고도 불린다.

`use`명령어로 데이터베이스에 접속한 후, `show tables` 명령을 이용하면 Database의 전체 테이블 목록을 볼 수 있다.<br>

```sql
show tables;
```

이때 존재하는 테이블 중 하나의 테이블의 구조를 확인하기 위해서는 `desc "테이블명"` 으로 확인할 수 있다.<br>
desc는 describe의 약자이다.<br>

```
desc testTable;
```
