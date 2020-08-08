---
title: "[Spring] 코드를 줄여주는 Lombok"
tags:
  - Spring
date: 2020-08-08T08:06:00-05:00
key: "[Spring] 코드를 줄여주는 Lombok"
---

# 코드를 줄여주는 Lombok

Lombok의 어노테이션을 이용하면 코드의 양을 줄여주고 코드의 수정이 필요할 때 쉽게 수정해 줄 수 있다.

<!--more-->

## @Getter, @Setter

Getter와 Setter가 필요한 클래스를 만들 때, 멤버를 private 지정자로 선언하고,<br>
변수를 get, set 할 수 있는 메서드를 일일이 만들었어야 했다.<br>

```java
public class Example {
    private Long id;
    private String title;
    private String Content;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getContent() {
        return Content;
    }

    public void setContent(String content) {
        Content = content;
    }
}
```

Lombok을 사용하면 이러한 일을 어노테이션으로 해결할 수 있다.<br>

```java
import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class Example {
    private Long id;
    private String title;
    private String Content;
}
```

멤버를 선언해 주고 어노테이션을 추가하고 그에 따른 import로 자동 생성해 준다.<br>

## Lombok으로 생성자 생성

생성자도 Lombok을 통해 생성할 수 있다.<br>

### NoArgsConstructor

```java
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter @Setter
@NoArgsConstructor
public class Example {
    private Long id;
    private String title;
    private String Content;
}
```

`@NoArgsConstructor`가 선언되었고 그에 따른 import가 되었다.<br>
`@NoArgsConstructor`는 말 그대로 인자가 없는 생성자, 즉 기본 생성자를 의미한다.<br>

### RequiredArgsConstructor

```java
public class Example {
    private Long id;
    private String title;
    private String Content;
    private final int finalNum;
}
```

`private final int finalNum`과 같이 반드시 지정된 값이 필요한 변수가 있다고 하자.<br>
이럴 땐 그 변수의 값을 지정하는 생성자가 필요하다.<br>
이때 `@RequiredArgsConstructor`을 이용하면 Lombok이 자동으로 생성자를 생산해 준다.<br>

```java
import lombok.Getter;
import lombok.RequiredArgsConstructor;
import lombok.Setter;

@Getter @Setter
@RequiredArgsConstructor
public class Example {
    private Long id;
    private String title;
    private String Content;
    private final int finalNum;
}
```

이때 final 변수가 스프링에 등록된 클래스라면, 자동으로 의존성 주입이 된다.
