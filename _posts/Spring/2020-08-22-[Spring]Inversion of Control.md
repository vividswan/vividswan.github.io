---
title: "[Spring] Inversion of Control"
tags:
  - Spring
date: 2020-08-22T08:06:00-05:00
key: "[Spring] Inversion of Control"
---

# Inversion of Control

제어의 역전(`Inversion of Control`) -> IoC에 대해 알아보자.

<!--more-->

## 제어

제어의 의미를 알아보자.<br>
한 클래스의 메서드에서 다른 구현체의 인스턴스를 만든다고 해보자.<br>

```java
public class InstanceClass {
    public void run(){
        System.out.println("run!");
    }
}
```

```java
public class Test {

    public static void setTest() {
        InstanceClass instanceClass = new InstanceClass();
        instanceClass.run();
    }

    public static void main(String[] args){
        setTest();
    }
}
```

Test라는 클래스에서 new 생성자를 이용해 인스턴스를 만들어주었다.<br>
이런 상황이 Test 클래스의 setTest() 메서드가 InstanceClass를 제어하고 있는 상황이다.<br>

```java
public class Test {

    public static void setTest() {
        InstanceClass instanceClass = new InstanceClass();
        instanceClass.run();
    }

    public static void setTest1() {
        InstanceClass instanceClass = new InstanceClass();
        instanceClass.run();
    }

    public static void setTest2() {
        InstanceClass instanceClass = new InstanceClass();
        instanceClass.run();
    }

    public static void setTest3() {
        InstanceClass instanceClass = new InstanceClass();
        instanceClass.run();
    }

    public static void main(String[] args){
        setTest();
        setTest1();
        setTest2();
        setTest3();
    }
}
```

만약에 이런 식으로 서로 다른 메서드에서 new 생성자를 이용해 인스턴스를 만든다면 서로 같은 인스턴스일까?<br>
답은 서로 다른 인스턴스이다.<br>
각 스택 영역에서 메서드가 호출되고 그 안에서 생성자를 이용해 heap 영역에서 인스턴스가 생성된다.<br>
이 heap 영역에서 생성된 인스턴스는 스택 영역이 소멸할 때 가비지 컬렉터에 의해 같이 소멸되므로, setTest,1,2,3의 인스턴스는 서로 다른 인스턴스이다.<br>

## Spring IoC

DB에 접근해야 하는 클래스들, 커넥션 풀, 스레드 풀과 같은 자원을 사용해야 하는 클래스들이 인스턴스를 여러 번 만들면, 자원의 낭비가 되고 예상할 수 없는 오류가 발생할 수 있다.<br>
이외에도 서버에서 사용되는 Service, Repository 객체도 중복으로 여러 번 만드는 것보다 인스턴스를 한 번만 생성해서 관리하는 것이 안정적인 방법이다.<br><br>

Spring에서는 Spring IoC(`Inversion of Control`)의 방식을 통해 이러한 관리를 가능하게 해준다.<br>
즉, 클래스를 생성하고, 의존성을 주입해 주는 제어를 Spring 쪽으로 역전시켜 스프링이 관리하는 것이다.<br>
스프링이 인식할 수 있는 어노테이션(@Component, @Repository, @Service 등등..)을 입력해 주면 스프링이 IoC로 관리해 준다.<br>
이때 생성된 인스턴스를 `싱글턴`으로 관리해 주므로 클래스가 만드는 인스턴스들이 중복되지 않고, 한 구현체에 한인 스터를 만들어줘서 관리하게 된다.<br>
그러므로 Spirng이 인식한 구현체들이 하나씩 heap 영역에 만들어져서 관리된다.<br>

## 예제

```java
import org.springframework.stereotype.Component;

@Component
public class InstanceClass {
    public void run(){
        System.out.println("run!");
    }
}
```

```java
import org.springframework.beans.factory.annotation.Autowired;

public class Test {
    @Autowired
    private InstanceClass instanceClass;

    public void test(){
        instanceClass.run();
    }
}
```

IoC가 인식할 수 있게 @Component와 같은 어노테이션을 붙여준다.<br>
이때 클래스의 위치는 스프링 부트 메인 클래스의 하위여야 스프링 IoC가 인식할 수 있다.<br>
IoC에 제어가 역전된 구현체는 위와 같은 @Autowired와 같은 방법으로 사용할 수 있다.<br>
또한 이 인스턴스는 싱글턴으로 관리되는 단 하나의 인스턴스다.<br>
