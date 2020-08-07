---
title: "[Java] Builder 패턴"
tags:
  - Java
last_modified_at: 2020-08-07T08:06:00-05:00
key: "[Java] Builder 패턴"
---

# Builder 패턴

생성자의 파라미터를 이용해 클래스를 만들 때 Builder 패턴을 사용한다.<br>

<!--more-->

## 필요성

생성자의 파라미터를 이용해 클래스를 만들 때, 신경 써야 할 인자들이 2~3개라면 상관없지만 인자들이 20개, 30개 .. 많은 수라고 생각해보자.<br>
그때그때 상황에 맞는 생성자를 일일이 만들어 줄 수 없기 때문에 Builder Pattern을 이용한다.<br>

## 예제

```java
public class Test {
    private int testEl1;
    private int testEl2;
    private int testEl3;
    private int testEl4;
    private int testEl5;
}
```

위와 같은 Test Class가 있다고 생각해보자.<br>
원하는 그때그때 원하는 변수만 바꿀 수 있는 생성자를 만들기 힘들다.<br>
이때 builder parttern을 사용한다.<br>

```java
public class Test {
    private int testEl1;
    private int testEl2;
    private int testEl3;
    private int testEl4;
    private int testEl5;

    public static class Builder {
        private final int testEl1;
        private final int testEl2;
        private int testEl3 = 0;
        private int testEl4 = 0;
        private int testEl5 = 0;

        public Builder(int testEl1, int testEl2){
            this.testEl1 = testEl1;
            this.testEl2 = testEl2;
        }

        public Builder testEl3(int testEl3){
            this.testEl3 = testEl3;
            return this;
        }

        public Builder testEl4(int testEl4){
            this.testEl4 = testEl4;
            return this;
        }

        public Builder testEl5(int testEl5){
            this.testEl5 = testEl5;
            return this;
        }

        public Test build() {
            return new Test(this);
        }
    }
    private Test(Builder builder){
        testEl1 = builder.testEl1;
        testEl2 = builder.testEl2;
        testEl3 = builder.testEl3;
        testEl4 = builder.testEl4;
        testEl5 = builder.testEl5;
    }
}
```

`testEl1`, `testEl2` 는 필수로 값을 지정해서 생성해야 하는 경우고, 나머지 인자들은 default 값을 정해주고 필요할 때 사용할 수 있는 방법이다.<br>
Test 클래스 안에 내부적으로 Builder 메서드를 만들어주고, return을 활용해 순차적으로 메서드를 불러서 인스턴스를 생성할 수 있다.<br>

```java
    public static void main(String[] args){
        Test test = new Builder(1,0).testEl3(5).testEl5(10).build();
    }
```

위와 같은 방법으로 필수적으로 들어가야 할 인자와 나머지는 선택적으로 사용하여 인스턴스를 생성할 수 있다.<br>

## 웹에서 사용

웹에서는 특정 클래스에 맹목적으로 Setter를 사용하면 어디서 값이 변했는지 알 수 없고 오류가 발생할 수 있다.<br>
이러한 문제를 Builder Pattern으로 해결할 수 있고, 스프링의 경우 Lombok을 활용하면 어노테이션 하나로 Builder를 구현할 수 있다.<br>
