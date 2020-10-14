---
layout: article
title: "[JPA] Pageable로 페이지 처리하기"
tags:
  - JPA
date: 2020-10-14-T08:06:00-05:00
key: "[JPA] Pageable로 페이지 처리하기"
---

# Pageable로 페이지 처리하기

Pageable을 이용해 JPA에서 페이지 단위로 데이터를 받아보자.<br>

<!--more-->

## TaskRepository

`Task`라는 모델을 받는 `TaskRepository`를 만들고, 페이지 단위로 Task를 받도록 해보자.<br>

```java
import com.vividswan.studymate.model.Task;
import org.springframework.data.jpa.repository.JpaRepository;

public interface TaskRepository extends JpaRepository<Task ,Long> {
}
```

우선 `JpaRepository`을 `Task` 타입과 `Task`의 PK 타입으로 제네릭 하여 상속받는 인터페이스를 작성하면, Task Entity에 대한 CRUD 메서드를 호출할 수 있는 `TaskRepository`가 만들어진다.<br>

여기서 페이지 단위로 데이터를 받기 위한 코드를 추가하자.<br>

```java
package com.vividswan.studymate.repository;

import com.vividswan.studymate.model.Task;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;

public interface TaskRepository extends JpaRepository<Task ,Long> {
    Page<Task> findAllByUserIdAndIsSuccess(Long userId, int isSuccess, Pageable pageable);
}
```

> findAllByUserIdAndIsSuccess<br>

Task Table에서 `userId`라는 속성과 `isSuccess`라는 속성을 입력받아 조회할 수 있는 메서드를 만들었다.<br>

> Page<Task><br>

return 값은 페이지 단위의 `Task` 타입이다.<br>

### 메서드 파라미터

> Long userId, int isSuccess, Pageable pageable<br>

메서드에 들어가는 파라미터를 확인해보자.<br>
userId 속성과 isSuccess 속성을 바탕으로 조회를 하기 때문에 이 두 값을 파라미터에 넣어줘야 한다.<br>
그 뒤에 가장 중요한 Page에 대한 설정값을 담아 줄 `Pageable pageable` 을 파라미터로 넣어줘야 페이지 처리를 할 수 있다.<br>

## TaskService

```java
package com.vividswan.studymate.service;

import com.vividswan.studymate.config.auth.PrincipalDetails;
import com.vividswan.studymate.model.Task;
import com.vividswan.studymate.repository.TaskRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.data.web.PageableDefault;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;


@Service
@RequiredArgsConstructor
public class TaskService2 {

    private final TaskRepository taskRepository;


    @Transactional(readOnly = true)
    public Page<Task> findList(PrincipalDetails principalDetails, @PageableDefault(size=8, sort = "deadline", direction = Sort.Direction.ASC) Pageable pageable) {
        return taskRepository.findAllByUserIdAndIsSuccess(principalDetails.getUserId(),0, pageable);
    }

}
```

다음과 같이 Service 계층에서 페이지 단위로 Task 객체를 불러올 수 있는 `findList` 메서드를 만들어주자.<br>

> @Transactional(readOnly = true)<br>

삽입, 수정, 삭제가 아닌 조회만 하는 메서드이므로 `readOnly` 트랜잭션을 선언해 준다.<br>

> PrincipalDetails principalDetails<br>

UserId를 출력하기 위해 선언했다.<br>
spring security context holder에 있는 세션 값을 통해 userId를 불러온다.<br>

### @PageableDefault

> @PageableDefault(size=8, sort = "deadline", direction = Sort.Direction.ASC) Pageable pageable<br>

@PageableDefault 어노테이션을 선언해 주고 page에 대한 설정을 할 수 있다.<br>
- size : 한 페이지에 담을 모델의 수를 정할 수 있다.<br>
- sort : 정렬의 기준이 되는 속성을 정할 수 있다.<br>
- direction : 오름차순과 내림차순 중 기준을 선택한다.<br>
- Pageable pageable : PageableDefault 값을 갖고 있는 변수를 선언한다.<br>

## 페이지 출력

```java
    @GetMapping("/todolist/proceeding")
    @ResponseBody
    public Page<Task> taskView(Model model, @AuthenticationPrincipal PrincipalDetails principalDetails, @PageableDefault(size=8, sort = "deadline", direction = Sort.Direction.ASC) Pageable pageable, int page){
        Page<Task> pagingTasks = taskService.findList(principalDetails, pageable);
        return pagingTasks;
    }
```
컨트롤러에서 @ResponseBody를 통해 데이터를 JSON 형태로 받아서 확인해보자.<br>

### 결과
```json
{
  "content": [
    {
      "id": 3,
      "title": "1234",
      "content": "<p>12345</p>",
      "createDate": "2020-10-13T15:31:32.965",
      "deadline": "2020-10-07T15:31:00",
      "user": {
        "id": 1,
        "username": "vividswan",
        "nickname": "수환",
        "email": "vividswan@naver.com",
        "password": "$2a$10$V.vNh2.zATNZqTeINZD57eGm6LD4C1ZxBJVah12MVZ6FBs1/tvKki",
        "role": "USER",
        "createDate": "2020-10-13T14:27:09.054"
      },
      "feedbacks": [
        
      ],
      "isSuccess": 0,
      "stringDeadline": "2020/10/07 03:31"
    }
  ],
  "pageable": {
    "sort": {
      "sorted": true,
      "unsorted": false,
      "empty": false
    },
    "pageNumber": 0,
    "pageSize": 8,
    "offset": 0,
    "paged": true,
    "unpaged": false
  },
  "totalPages": 1,
  "totalElements": 1,
  "last": true,
  "numberOfElements": 1,
  "first": true,
  "size": 8,
  "number": 0,
  "sort": {
    "sorted": true,
    "unsorted": false,
    "empty": false
  },
  "empty": false
}
```

위와 같이 페이지 처리되어 결과가 나옴을 확인할 수 있다.<br>
JSON에 포함된 `last`, `first`, `pageNumber`, `pageSize` 등의 데이터를 화면을 구현할 때 활용할 수 있다.<br>