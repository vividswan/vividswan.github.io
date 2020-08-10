---
layout: article
title: "[Database] MySQL Update & Delete"
tags:
  - Database
date: 2020-08-11T08:06:00-05:00
key: "[Database] MySQL Update & Delete"
---

# MySQL Update & Delete

MySQL로 자료를 수정(Update)하고 삭제(Delte)해보자.

<!--more-->

## Update

> show tables;

데이터베이스를 `use` 한 뒤 사용할 테이블을 확인한다.<br>

![1](/assets/images/200811-1.png)<br>

> SELECT \* from testTable;

testTable이 갖고 있는 자료들을 조회한다.<br>

![2](/assets/images/200811-2.png)<br>

### Update 명령어

> UPDATE "테이블 이름" SET "column1 이름" = "column1 값", "column2 이름" = "column2 값" ... where "column 이름" = "column1 값";

위와 같은 명령어로 데이터의 자료를 수정할 수 있다.<br>
**이때, where로 특정 값을 지정해 주지 않으면 테이블의 모든 값이 변한다.**<br>

> UPDATE testTable SET content='hello world!', title='title' where id = 1;

이렇게 쿼리를 날리면,<br>

![3](/assets/images/200811-3.png)

위와 같이 자료가 변한다.<br>

## Delete

삭제는 수정과 다르게 따로 column에 대한 value를 정해주지 않아도 되기 때문에 간단하다.<br>
**삭제도 반드시 where로 특정 값을 지정해 주어야 한다.**<br>

> Delete from "테이블 이름" where "column 이름" = "column1 값";

위와 같은 명령어로 데이터의 자료를 삭제할 수 있다.<br>

> Delete testTable where id=4;

`testTable`에서 `id=4`인 자료를 지우는 명령어이다.<br>

![4](/assets/images/200811-4.png)

위와 같이 4번이 지워진 것을 확인할 수 있다.<br>

## CRUD

앞선 게시글까지 해서 MySQL을 이용하여 CRUD 조작 방법을 살펴보았다.<br>
CRUD는 C(Create) R(Read) U(Update) D(Delete)의 약자이며 모든 데이터베이스의 기본이라고 할 수 있다.<br>
