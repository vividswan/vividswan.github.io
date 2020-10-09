---
layout: article
title: "[JPA] ORM(Object Relational Mapping)"
tags:
  - JPA
date: 2020-10-10-T08:06:00-05:00
key: "[JPA] ORM(Object Relational Mapping)"
---

# ORM(Object Relational Mapping)

객체지향 언어로 관계형 데이터 베이스를 이용해 개발할 때 필요한 ORM을 알아보자.<br>

<!--more-->

## ORM 이란?

`Object Relational Mapping` : 객체 관계 매핑<br>
이름 그대로 객체를 관계형 데이터 베이스에 맞게 매핑해주는 것이다.<br>
더 풀어서 얘기하면 객체지향과 관계형 데이터베이스의 패러다임 불일치를 해결해 주고 객체로 데이터 베이스 모델을 만들어주는 것이다.<br>

## 객체지향과 관계형 데이터베이스의 패러다임 불일치

웹사이트를 만든다고 가정해보자.<br>
웹사이트를 이용할 User가 있고, 그 User는 웹사이트에서 자기가 관리해야 하는 일(Task)이 필요하다고 하자.<br>
RDB에선 User Table과 Task Table을 만들고, Task 테이블에서 User의 `PK(Primary Key)`를 `FK(Foreign Key)`로 저장하여 필요할 때 Join을 통해 Task를 작성한 유저를 조회할 수 있다.<br>
객체지향에서는 User와 Task에 대한 객체를 어떻게 만들까?<br>

### User.java
```java
public class User{
  private Long id;
  // ...
}
```

### Task.java
```java
public class Task{
  private Long id;
  private User user;
  // ...
}
```

RDB에선 객체 자체를 담을 수 없기 때문에 FK로 참조해야 하지만, 객체지향에선 위의 코드처럼 User 객체를 객체 자체로 레퍼런스에 의한 참조를 할 수 있다.<br>
이런 객체지향과 관계형 데이터베이스의 패러다임 불일치를 해결하기 위해 ORM을 사용한다.<br>

## ORM의 사용

ORM이 등장하기 전 기존의 방식은 먼저 데이터 베이스의 테이블을 모델링 한 후 그에 맞춰서 객체지향에서 코드로 구현했다.<br>
그러므로 위처럼 레퍼런스로 참조하는 방식이 아닌, User 테이블의 FK를 Task 객체에 저장해 주었다.<br>

### RDB에 맞춰진 객체 설계

```java
public class Task{
  private Long id;
  private Long user_id;
  // ...
}
```

객체지향적이지 못한 코드가 발생하게 된 것이다.<br>
ORM은 Object Relational Mapping, 말 그대로 객체를 관계형 데이터베이스에 매핑해주는 것이기 때문에 기존의 방식처럼 데이터 베이스의 테이블을 만드는 것이 아니다.<br>
객체지향에서 Entity를 먼저 설계하면, ORM이 그에 맞춰 RDB의 테이블을 만들어준다.<br>

## JPA 예제

JPA에서 User와 Task 객체를 만들고, 데이터베이스에 그에 매핑된 FK를 갖고 있는 테이블이 만들어지는지 확인하자.<br>

### User.java

```java
package com.vividswan.studymate.model;

import lombok.*;
import org.hibernate.annotations.CreationTimestamp;
import org.springframework.data.annotation.CreatedDate;

import javax.persistence.*;
import java.sql.Timestamp;
import java.time.LocalDateTime;

@NoArgsConstructor
@AllArgsConstructor
@Getter
@Builder
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 100, unique = true)
    private String username;
    @Column(nullable = false, length = 100)
    private String nickname;
    @Column(nullable = false, length = 50)
    private String email;
    @Column(nullable = false, length = 100)
    private String password;

    @Enumerated(EnumType.STRING)
    private RoleType role;

    @CreationTimestamp
    private LocalDateTime createDate;

    public void update(String password, String nickname, String email){
        this.password = password;
        this.nickname = nickname;
        this.email=email;
    }
}
```

### Task.java

```java
package com.vividswan.studymate.model;

import lombok.*;
import org.hibernate.annotations.CreationTimestamp;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.format.annotation.DateTimeFormat;

import javax.persistence.*;
import java.sql.Timestamp;
import java.time.LocalDateTime;

@NoArgsConstructor
@AllArgsConstructor
@Builder
@Getter
@Entity
public class Task {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false,length = 100)
    private String title;

    @Lob
    private String content;

    @CreationTimestamp
    private LocalDateTime createDate;

    private LocalDateTime deadline;

    @ManyToOne
    @JoinColumn(name="userId")
    private User user;

    private int isSuccess;
}
```
다음과 같이 User와 Task 객체를 작성했다.<br>
Task 코드에서 User를 참조하는 부분을 확인해보자.<br>

```java
//...
    @ManyToOne
    @JoinColumn(name="userId")
    private User user;
//...
```
어노테이션을 간단히 살펴보자면,<br>
> @ManyToOne : 다수의 Task가 한 명의 User에 의해 작성될 수 있으므로 Many(Task)toOne(User)이다.<br>
> @JoinColumn(name="userId") : 관계형 데이터 베이스 테이블을 작성할 때 칼럼에 이름은 `userId`인 user를 참조할 수 있는 PK를 생성해달라는 것이다.<br> 

`private User user;`에서 알 수 있듯이, 객체지향의 설계에 맞게 PK가 아닌 객체 래퍼런스로 참조하고 있다.<br>

### 데이터베이스에서 확인
관계형 데이터베이스에 잘 매핑이 되었는지 확인해보자.<br>

![1](/assets/images/201010-1.png)<br>

User에 대한 PK가 매핑됐음을 확인할 수 있다.<br>