---
title: "[Database] MySQL - Join"
tags:
  - Database
date: 2020-10-22T08:06:00-05:00
key: "[Database] MySQL - Join"
---

# MySQL - Join

MySQL에서 사용하는 Join(Inner Join, Equi Join, Outer Join)을 알아보자.<br>

<!--more-->

## Tables

아래와 같은 두 개의 테이블로 Join을 알아보자.<br>

![1](/assets/images/201022-1.png)<br>
![2](/assets/images/201022-2.png)<br>

## Inner Join

테이블이 두 개 있을 때, 두 집합의 교집합 영역을 출력하는 것이 Inner Join이다.<br>

다음과 같이 입력해보자<br>

> select * from students s join teachers t on s.tid = t.tid; 

![3](/assets/images/201022-3.png)<br>

students와 teachers 테이블에서 tid가 동일한(교집합 영역) 튜플을 출력한다.<br>

## Equi Join
Equi Join은 두 테이블 사이에서 attribute의 값이 같은 튜플을 출력하는 Join이고, 같지 않는 걸 출력하는 건 Non-Equi Join이다.<br>

> select * from students s, teachers t where s.tid = t.tid;

## Outer Join

### Left Join

### Right Join