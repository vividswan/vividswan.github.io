---
title: "[Spring] MockMvc - 간단한 테스트 코드"
tags:
  - Spring
date: 2020-08-13T08:06:00-05:00
key: "[Spring] MockMvc - 간단한 테스트 코드"
---

# MockMvc

MockMvc를 이용한 간단한 테스트 코드를 만들어보자.

<!--more-->

## Controller

```java
package com.vividswan.spring.web;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class PracticeController {

    @GetMapping("/practice")
    public String practice(){
        return "hello world!";
    }
}
```

`/practice`로 get 요청을 하였을 때,<br>
`hello world!`를 띄우게 해주는 간단한 컨트롤러를 만든다.<br>

> @RestController는 객체를 return 하면 JSON 데이터 형식으로 반환해 주는 컨트롤러이다.

여기선 간단하게 "hello world"라는 String 자료를 화면에 띄워준다.

## Test Code

```java
package com.vividswan.spring.web;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;


@RunWith(SpringRunner.class)
@WebMvcTest(controllers=PracticeController.class)
public class PracticeControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void practice_테스트() throws Exception{
        String data = "hello world!";

        mvc.perform(get("/practice"))
                .andExpect(status().isOk())
                .andExpect(content().string(data));
    }
}
```

```java
    @Autowired
    private MockMvc mvc;
```

`Autowired`로 `MockMvc`를 `mvc`라는 변수에 의존성 주입한다.<br>

```java
    @Test
    public void practice_테스트() throws Exception{
        String data = "hello world!";

        mvc.perform(get("/practice"))
                .andExpect(status().isOk())
                .andExpect(content().string(data));
    }
```

> @Test

test를 위한 메서드에는 항상 @Test 어노테이션을 붙여준다.<br>
"hello world"라는 data String과 `/practice`로 get 요청됐을 때 응답한 내용이 같은지 확인하는 과정이다.<br>

> mvc.perform(get("/practice"))

MockMvc를 이용해 `/practice`로 get 요청을 보낸 상황을 만든다.<br>

> andExpect(status().isOk())

연결 상태가 OK(`200번대`)인지 확인한다.<br>

> andExpect(content().string(data));

응답한 내용이 data의 값과 같은지 확인한다.<br>

## Test 결과 확인

![1](/assets/images/200813-1.png)<br>
테스트를 돌려보고 성공하면 위와 같은 초록 버튼과 메시지가 출력된다.<br>
테스트에 실패하면 빨간 버튼이 나온다.<br>
