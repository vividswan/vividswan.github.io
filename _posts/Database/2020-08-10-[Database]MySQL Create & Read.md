---
layout: article
title: "[Database] MySQL Create & Read"
tags:
  - Database
date: 2020-08-10T08:06:00-05:00
key: "[Database] MySQL Create & Read"
---

# MySQL Create & Read

MySQL로 자료를 저장하고 읽어보자.

<!--more-->

## Create

> Insert Into Table_Name (column1, column2 .. ) values (value1, value2, value3 ... )

MySQL은 위와 같은 문법으로 테이블 안에 ROW 데이터를 저장할 수 있다.<br>

> DESC (Table 이름)

`DESC`(Describe)를 이용하면 현재 테이블의 구조를 볼 수 있다.<br>

![1](/assets/images/200810-1.png)<br>

위 구조에 맞춰서 자료를 생성하면 된다.<br>

> Insert INTO testTable (title,content,created,author,profile) VALUES('hello','world...',NOW(),'vividswan','student');

- id는 `Auto_Incresment` 옵션이 있으므로 생략
- created는 `NOW()`라는 함수를 이용하면 생성되는 시점의 시간이 들어간다.
- 각각의 column 이름과 VALUES와의 순서가 맞아야 된다.

> Query OK, 1 row affected (0.05 sec)

자료가 정확히 생성됐다면 위와 같은 메세지가 나온다.<br>

## Read

> SELECT (column1, column2 .. ) FROM (Table Name);

SELECT와 FROM 사이에 조회하고 싶은 column 이름을 쓰고 FROM 뒤에 테이블 이름을 입력하면 테이블에서 결과가 출력된다.<br>
이때, column 이름 자리에 `*`(와일드카드)를 쓰면 모든 column의 자료가 출력된다.<br>

> SELECT \* FROM testTable;

![2](/assets/images/200810-2.png)<br>

### Read의 다른 기능들

#### where

> SELECT \* FROM testTable where author='vividswan'

testTable에서 author가 'vividswan'인 경우만 조회된다.<br>

#### ORDER BY

> SELECT \* FROM testTable ORDER BY id DESC;

id를 기준으로 내림차순(Descending) 되어서 출력된다.<br>
`DESC` 대신 `asc`를 사용하면 오름차순이 출력된다.<br>

#### LIMIT

> SELECT \* FROM testTable LIMIT "Number"

`Number` 만큼의 자료가 출력된다. MySQL의 자료수가 많아서 출력이 어려울 때 사용한다.<br>

#### 조건을 다 같이 사용

> SELECT \* FROM testTable where author='vividswan' ORDER BY id DESC LIMIT 5

위와 같이 여러 조건들을 이용해서 조회할 수도 있다.<br>
