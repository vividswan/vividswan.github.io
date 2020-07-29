---
layout: article
title: "[Database] JDBC"
tags:
  - Database
date: 2020-07-29T08:06:00-05:00
key: "[Database] JDBC"
---

# JDBC

<!--more-->

JAVA로 DBMS를 조회하고 조작할 수 있는 JDBC에 대해서 알아보자.

## JDBC란?

- Java Database Connectivity의 약자
- 자바 언어와 데이터베이스를 연결해 주는 통로
- 자바를 이용한 데이터베이스 접속, SQL 문장의 실행, 얻어진 데이터의 핸들링을 위한 규약
- 자바 안에서 SQL 문을 실행시키기 위한 자바 API
- SQL과 프로그래밍언어의 통합 접근
- 인터페이스가 이미 정의돼있기 때문에, 데이터베이스의 밴더와 상관없이 동일하게 사용

## JDBC의 설치

- lib의 디렉터리에 라이브러리를 추가
- Maven 환경에서는 의존성(dependency)을 추가

## JDBC 프로그래밍 단계

- java.sql을 import
- 드라이버를 로드(Class.forName(~));
- DriveManager로 Connection 객체를 얻음(`DriverManager.getConnection(dburl, ID, Password)`)
- Connection 객체로 Statement 객체를 생성(`Connection객체.createStatement()`);
- statement를 수행한 결과로 ResultSet을 얻음 (`statement객체.executeQuery("쿼리 문장")`)
- 결과를 받고 모두 Close로 해제(`순서를 거꾸로 Close`)

## 코드 예제

```java
public static Connection getConnection() throws Exception{
	String url = "데이터베이스에 맞는 dburl이 들어간다.";
	String user = "userID";
	String password = "userPassword";
	Connection conn = null;
	Class.forName("로드할 드라이버의 정보를 입력");
	conn = DriverManager.getConnection(url, user, password);
	return conn;
}
Statement stmt = con.createStatement();
```

## 쿼리문

```java
stmt.execute(“query”);
stmt.executeQuery(“query”);
//SELECT 할 때 사용
stmt.executeUpdate(“query”);
//INSERT, UPDATE, DELETE 할 때 사용
```

## 결과 받기 예제

```java
ResultSet rs =  stmt.executeQuery( "select data from db" );
while ( rs.next() )
      System.out.println( rs.getInt( "data") );
```

## close(반대로)

```java
rs.close();
stmt.close();
con.close();
```

try 문을 사용해서 닫아주자.
