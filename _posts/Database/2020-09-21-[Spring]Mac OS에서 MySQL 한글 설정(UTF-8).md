---
title: "[Spring] Mac OS에서 MySQL 한글 설정(UTF-8)"
tags:
  - Spring
date: 2020-09-21T08:06:00-05:00
key: "[Spring] Mac OS에서 MySQL 한글 설정(UTF-8)"
---

# MySQL - UTF8 세팅

terminal을 이용해서 MySQL의 인코딩 방식을 UTF-8로 설정하자.<br>

<!--more-->

## 한글 설정(UTF-8)

terminal을 실행하여 설정할 수 있다.<br>

## 파일 복사

> $ sudo cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf

## 파일 편집 실행

> $ sudo vi /etc/my.cnf

`i`로 insert 모드로 들어가 아래의 내용을 붙여넣어준다.<br>

```sql
[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci

init_connect=SET collation_connection=utf8_general_ci
init_connect=SET NAMES utf8

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
```

붙여넣기 직후 `esc` 클릭 -> :wq! 입력 후 엔터를 누르면 저장된다.<br>

## MySql 접속

> mysql -u root -p

## UTF-8 확인

> mysql> show variables like 'c%';

![1](/assets/images/200921-1.png)

위와 같은 결과를 확인할 수 있다.<br>