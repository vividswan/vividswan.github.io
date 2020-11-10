---
title: "[Spring] 서버 실행 시 DB Table 생성"
tags:
  - Spring
date: 2020-11-09T08:06:00-05:00
key: "[Spring] 서버 실행 시 DB Table 생성"
---

# 서버 실행 시 DB Table 생성

ApplicationRunner을 구현하는 객체를 만들어서 서버 실행 시 DB Table을 생성해보자.<br>

<!--more-->

## DatabaseConfig

config 패키지를 만들고 DatabaseConfig를 다음과 같이 작성해 주자.<br>

```java
import lombok.RequiredArgsConstructor;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

@RequiredArgsConstructor
@Component
public class DatabaseConfig implements ApplicationRunner {

    private final DataSource dataSource;

    @Override
    public void run(ApplicationArguments args) throws SQLException {
        try(Connection connection = dataSource.getConnection()){
            String sql = "create table if not exists users(" +
                    "id bigint(5) not null auto_increment," +
                    "email varchar(100) not null," +
                    "password varchar(100) not null," +
                    "primary key(id));";
            Statement statement = connection.createStatement();
            statement.executeUpdate(sql);
        } catch (Exception e){
            System.out.println(e);
        }
    }
}
```

### DataSource

JDBC로 데이터를 접근하는 Connection을 관리하는 객체이다.<br>
DataSource를 이용하면 미리 생성된 Connection을 제공받을 수 있으므로 DB에 더 효율적이고 빠르게 접근이 가능하다.<br>
final로 선언하고 롬복의 `@RequiredArgsConstructor`를 이용해 Setter 주입으로 사용한다.<br>

### ApplicationRunner

'ApplicationRunner'의 `run()` 메서드는 애플리케이션 구동 시 바로 실행된다.<br>

### DB 연결 후 테이블 생성
`run()` 메서드 안의 내용은 sql 문을 이용해 예제 Table을 생성하는 내용이다.<br>

## DB Table

서버 실행과 동시에 아래의 테이블이 생성된 것을 확인할 수 있다.<br>
![1](/assets/images/201109-1.png)<br>
