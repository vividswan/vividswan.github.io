---
title: "[Spring] Post 요청 테스트 코드 작성"
tags:
  - Spring
date: 2020-09-27T08:06:00-05:00
key: "[Spring] Post 요청 테스트 코드 작성"
---

# Post 요청 테스트 코드 작성

Post 요청으로 사용자를 등록하는 테스트 코드를 작성해보며 Junit 5 테스트 코드를 알아보자.<br>

<!--more-->

JPA를 이용해 MySQL에 User를 등록하는 코드를 다음과 같이 작성했다.<br>

## model/User.java

```java
package com.vividswan.studymate.model;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.hibernate.annotations.CreationTimestamp;

import javax.persistence.*;
import java.sql.Timestamp;

@NoArgsConstructor
@AllArgsConstructor
@Data
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
    private Timestamp createDate;
}
```

## controller/userController
```java
package com.vividswan.studymate.controller;

import com.vividswan.studymate.dto.UserJoinDto;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class UserController {
    @GetMapping("join")
    public String joinForm(){
        return "user/joinForm";
    }

}
```

## service/UserService

```java
package com.vividswan.studymate.service;

import com.vividswan.studymate.dto.UserJoinDto;
import com.vividswan.studymate.model.User;
import com.vividswan.studymate.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.transaction.Transactional;

@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    @Transactional
    public void join(UserJoinDto userJoinDto) {
        userRepository.save(userJoinDto.toEntity());
    }
}
```

## repositoey/UserRepository

```java
package com.vividswan.studymate.repository;

import com.vividswan.studymate.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

위와 같은 model, controller, service, repository를 작성해준다.<br>
그 다음 클라이언트단에서 유저 정보를 보내줄 js 파일과 스프링에서의 api controller를 작성해준다.<br>

## static/js/user.js

```javascript
let index ={
    init : function() {
        $("#btn-join").on("click",()=>{
            this.save();
        });
    },
    save : function(){
        let data = {
            username:$("#username").val(),
            password:$("#password").val(),
            email:$("#email").val(),
            nickname:$("#nickname").val(),
        };


        $.ajax({
            type:"POST",
            url:"/api/joinProc",
            data:JSON.stringify(data),
            contentType:"application/json; charset=utf-8"
        })
        .done(function(response){
            if(response.status === 500){
                alert("로그인에 실패하였습니다.");
            }
            else{
                alert("로그인에 성공하였습니다.");
            }
            location.href="/";
        }).fail(function (error){
            alert(JSON.stringify(error));
        });
    }
}
index.init();
```

## api/UserApiController

```java
package com.vividswan.studymate.api;

import com.vividswan.studymate.dto.UserJoinDto;
import com.vividswan.studymate.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequiredArgsConstructor
public class UserApiController {

    private final UserService userService;

    @PostMapping("/api/joinProc")
    public int joinProc(@RequestBody UserJoinDto userJoinDto){
        userService.join(userJoinDto);
        return HttpStatus.OK.value();
    }
}
```

위의 `UserApiController`에 대한 테스트 케이스를 작성해보자.<br>

## UserApiContollerTest.java

```java
package com.vividswan.studymate.api;

import com.vividswan.studymate.dto.UserJoinDto;
import com.vividswan.studymate.model.User;
import com.vividswan.studymate.repository.UserRepository;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

import java.util.List;


@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserApiControllerTest {

    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private UserRepository userRepository;

    @AfterEach
    public void tearDown() throws Exception{
        userRepository.deleteAll();
    }

    @Test
    public void joinTest() throws Exception{
        //given
        String username = "vividswan";
        String password = "1234";
        String email = "vividswan@naver.com";
        String nickname = "soohwan";
        UserJoinDto userJoinDto = UserJoinDto.builder()
                .username(username)
                .password(password)
                .email(email)
                .nickname(nickname)
                .build();

        String url = "http://localhost:"+port+"/api/joinProc";

        //when
        ResponseEntity<Long> responseEntity = restTemplate.postForEntity(url,userJoinDto,Long.class);

        //then
        Assertions.assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
        Assertions.assertThat(responseEntity.getBody()).isGreaterThan(0L);

        List<User> all = userRepository.findAll();
        Assertions.assertThat(all.get(0).getUsername()).isEqualTo(username);
        Assertions.assertThat(all.get(0).getEmail()).isEqualTo(email);
    }

}
```

> @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)<br>

스프링부트로 동작하는 테스트임을 선언하고, 랜덤한 포트를 사용할 수 있는 어노테이션이다.<br>

> @Autowired<br>
    private TestRestTemplate restTemplate;

`ResponseEntity`로 포스트 요청을 하기 위해 TestRestTemplate를 이용한다.<br>

>   @AfterEach<br>
    public void tearDown() throws Exception{ <br>
        userRepository.deleteAll(); <br>
    }<br>

각 테스트가 끝나면 `repository`를 초기화 할 수 있게 @AfterEach로 선언한 뒤 데이터베이스를 지워주도록 한다.<br>

> //when<br>
        ResponseEntity<Long> responseEntity = restTemplate.postForEntity(url,userJoinDto,Long.class);

TestRestTemplate로 랜덤 포트의 url에 dto를 보내주고 Long.class의 반환값을 받는다.<br>

> Assertions.assertThat

`Assertions.assertThat`으로 연결상태와 자료의 일치 여부를 확인한다.<br>
이 때 Assertions는 Junit이 아닌 `assertj`로 임폴트하여 사용한다.<br>

```java
import org.assertj.core.api.Assertions;
```