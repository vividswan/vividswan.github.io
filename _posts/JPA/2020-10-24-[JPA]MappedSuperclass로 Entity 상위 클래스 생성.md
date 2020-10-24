---
layout: article
title: "[JPA] MappedSuperclass로 Entity 상위 클래스 생성"
tags:
  - JPA
date: 2020-10-24-T08:06:00-05:00
key: "[JPA] MappedSuperclass로 Entity 상위 클래스 생성"
---

# MappedSuperclass로 Entity 상위 클래스 생성

@MappedSuperclass를 이용하여 Entity들의 상위 클래스를 생성하여 중복 코드를 줄여보자.<br>

<!--more-->


## @CreatedDate

Model 객체의 Entity를 생성할 때, 생성된 시간을 저장하고 싶으면 다음과 같은 속성을 Entity에 추가하면 된다.<br>

```java
    @CreatedDate
    private LocalDateTime createDate;
```

Entity에 다음과 같은 속성을 추가하고, 이 Entity를 생성하면, DB에 다음과 같은 칼럼이 저장된다.<br>

![1](/assets/images/201024-1.png)

해당 데이터를 만든 시간이 DB에 저장된다.<br><br>

생성 시간이 필요 없는 Entity가 있을까??<br>
아마 웬만한 Entity는 모두 생성 시간이 필요할 것이다.<br>
그렇다고 똑같은 @CreateDate 코드를 각각 Entity마다 입력하면 비효율적이다.<br>
`@MappedSuperclass`로 Entity 상위 클래스 생성해서 코드의 반복을 없애보자.<br>

## CreateDateEntity

다음과 같이 CreateDateEntity를 작성해 준다.<br>

```java
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import javax.persistence.EntityListeners;
import javax.persistence.MappedSuperclass;
import java.time.LocalDateTime;

@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class CreateDateEntity {
    @CreatedDate
    private LocalDateTime createDate;
}
```

`@MappedSuperclass` 주석으로 인해 이 클래스를 다른 Entity가 상속할 경우, createDate 필드를 사용할 수 있다.<br>
`@EntityListeners(AuditingEntityListener.class)`는 Auditing 기능을 사용  해주는 어노테이션인데, Auditing은 JPA가 DB의 변화가 있을 때마다 감시해 주는 기능으로, 새로운 데이터가 생성될 때마다 createDate에 시간을 넣어야 하므로 사용해야 한다.<br>

## 상속, Auditing 활성화

기존에 User에 있던 `createDate` 필드를 지워주고 다음과 같이 상속한다.<br>

> public class User extends CreateDateEntity


### Application Class

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@EnableJpaAuditing // Auditing 활성화
@SpringBootApplication
public class StudyMateApplication {

	public static void main(String[] args) {
		SpringApplication.run(StudyMateApplication.class, args);
	}

}
```

Application Class에 `@EnableJpaAuditing`를 추가해서 Auditing 기능을 활성화시킨다.<br>

## 결과 확인

![2](/assets/images/201024-2.png)<br>

상속받기 전과 동일하게 생성 시간이 출력된다.<br>
이로써 createDate 필드가 필요한 모든 Entity는 `CreateDateEntity`를 상속받으면 코드의 중복을 피할 수 있다.<br>