---
title: "[Spring] 회원가입 DTO 만들기"
tags:
  - Spring
date: 2020-10-31T08:06:00-05:00
key: "[Spring] 회원가입 DTO 만들기"
---

# User Join DTO(Data transfer Object) 생성

회원가입에 필요한 DTO를 만들어 회원 등록을 해보자.<br>

<!--more-->

## DTO(Data transfer Object)

DTO는 데이터를 전송하기 위해 사용하는 객체이다.<br>
DAO(Data Access Object)와는 다르게 작업을 전담하는 메서드를 만들지 않기 때문에, Getter/Setter와 생성자 외에는 별도의 메서드를 정의하지 않는다.<br>


## Entity 객체

다음과 같은 Entity 모델이 있다고 하자.<br>

```java
import lombok.*;

import javax.persistence.*;

@NoArgsConstructor
@AllArgsConstructor
@Getter
@Builder
@Entity
public class User  extends CreateDateEntity{
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

    public void update(String password, String nickname, String email){
        this.password = password;
        this.nickname = nickname;
        this.email=email;
    }
}
```

회원가입을 위해선 클라이언트에게 회원가입에 필요한 정보를 받고 이 정보를 이용해서 회원 정보를 DB에 저장해 준 뒤 클라이언트에게 응답을 해줘야 한다.<br>
이때, 클라이언트에 대한 요청과 응답을 Entit 객체인 User.java를 사용하면 안 된다.<br>
Entity 객체는 DB와 맞닿은 객체이기 때문에 여러 클래스에 영향을 줄 수 있기 때문에 빈번한 정보 등록과 수정을 하기 위해선 DTO를 만들어줘서 역할을 분리해 주도록 하자.<br>

### UserJoinDto

```java
import com.vividswan.studymate.model.RoleType;
import com.vividswan.studymate.model.User;
import lombok.*;

@Data
@NoArgsConstructor
public class UserJoinDto {
    private String username;
    private String password;
    private String email;
    private String nickname;
    private RoleType role;

    @Builder
    public UserJoinDto(String username, String password, String email, String nickname, RoleType role){
        this.username = username;
        this.password = password;
        this.email = email;
        this.nickname = nickname;
        this.role = role;
    }

    public User toEntity(){
        return User.builder()
                .username(username)
                .password(password)
                .email(email)
                .nickname(nickname)
                .role(role)
                .build();
    }
}
```

회원가입에서 클라이언트가 보낸 정보를 전달하는 DTO를 다음과 같이 만들고 코드를 살펴보자.<br>

```java
    private String username;
    private String password;
    private String email;
    private String nickname;
    private RoleType role;
```
User에게 직접 받는 정보를 속성으로 선언했다.<br>

```java
    @Builder
    public UserJoinDto(String username, String password, String email, String nickname, RoleType role){
        this.username = username;
        this.password = password;
        this.email = email;
        this.nickname = nickname;
        this.role = role;
    }
```
`Builder` 패턴을 이용하여 Dto를 생성할 수 있는 생성자를 만들어준다.<br>

```java
    public User toEntity(){
        return User.builder()
                .username(username)
                .password(password)
                .email(email)
                .nickname(nickname)
                .role(role)
                .build();
    }
```
User에게 받은 정보와 Builder 생성자를 이용하여 Dto 객체를 만들고, 이를 이용하여 DB에 삽입할 Entity 객체를 만들어 리턴해주는 toEntity 메서드이다.<br>

## Dto를 이용한 회원가입

```java
    @Transactional
    public boolean join(UserJoinDto userJoinDto) {
        String rawPassword = userJoinDto.getPassword();
        String encPassword = bCryptPasswordEncoder.encode(rawPassword);
        userJoinDto.setPassword(encPassword);
        userJoinDto.setRole(RoleType.USER);
        userRepository.save(userJoinDto.toEntity());
        return true;
    }
```

Service 층에서 가입할 회원정보를 Repository 층에 전달해 준다.<br>
트랜잭션 처리를 보장하기 위해 `@Transactional` 어노테이션을 사용해야 한다.<br>
클라이언트로부터 넘어온 dto에서 비밀번호의 해시 암호화를 해주고, User의 Role을 지정한 뒤, toEntity로 Dto의 정보를 바탕으로 생성된 Entity 객체를 리턴 받은 뒤, Repository 층에서 DB에 회원정보를 저장해 준다.<br>