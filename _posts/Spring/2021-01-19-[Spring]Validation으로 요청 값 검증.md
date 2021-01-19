---
title: "[Spring] Validation으로 요청 값 검증"
tags:
  - Spring
date: 2021-01-19T08:06:00-05:00
key: "[Spring] Validation으로 요청 값 검증"
---

# Validation으로 요청 값 검증

클라이언트가 요청 값을 보낼 때, Validation을 이용하여 서버에서 올바른 값인지 검증해보자.<br>

<!--more-->

## 의존성 추가

gradle을 이용한다면 build.gradle에 다음과 같은 의존성을 추가해 준다.<br>

```gradle
    compile group: 'javax.validation', name: 'validation-api'
    compile group: 'org.hibernate', name: 'hibernate-validator', version: '6.1.0.Final'
```

## Dto 작성

클라이언트에게 제공받은 데이터를 처리할 `SignupDto`를 다음과 같이 작성한다.<br>

```java
import lombok.*;
import org.hibernate.validator.constraints.Length;

import javax.validation.constraints.Email;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.Pattern;


@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class SignupDto {

    @NotBlank
    @Length(min=3, max = 20)
    @Pattern(regexp = "^[ㄱ-ㅎ가-힣a-zA-Z0-9_-]{3,20}$")
    private String nickname;

    @NotBlank
    @Length(min=8, max= 50)
    private String password;

    @Email
    @NotBlank
    private String email;
}
```

> @NotBlank : 공백을 방지하는 주석이다.<br>
> @Length(min=a, max = b) : 최소 a, 최대 b 범위로 길자 길이를 제한한다.<br>
> @Pattern(regexp = "") : 정규 표현식을 사용하여 들어올 수 있는 문자를 제한한다.<br>
> 위의 예제에선 한글, 영어 대소문자, 숫자, '_','-'를 허용한다.<br>
> @Email : String 형식의 데이터가 Email 형식인지 확인한다.<br>

## 컨트롤러에서 검증

클라이언트가 "/sing-up" 경로로 Post 요청을 통해 form의 정보를 보내준다면 다음과 같이 처리할 수 있다.

```java
    @PostMapping("/sign-up")
    public ResponseEntity<?> singUpRequest(@RequestBody @Valid SignupDto signupDto, Errors errors){
        return accountService.signUp(signupDto, errors);
    }
```

파라미터에 `@Valid`를 선언해서 요청 값을 검증할 수 있고, 검증 값이 어긋나면 `Errors errors`로 처리할 수 있다.<br>
위의 예제에서는 서비스 레이어에 Errors 객체를 전달했다.<br>

## Iterator로 에러 메시지 추출

서비스에서 전달받은 Errors를 이터레이터로 반환받아 확인해보자.<br>

```java
   private List<String> validCheck(SignupDto signupDto, Errors errors){
        List<String> errorList = new ArrayList<>();

        Iterator<ObjectError> it = errors.getAllErrors().iterator();
        while(it.hasNext()){
            errorList.add(it.next().getDefaultMessage());
        }

        return errorList;
    }
```

## 에러 메시지 확인해보기

다음과 같이 Validation에 어긋나는 데이터를 전달하면 각각의 오류 메시지가 출력되는 것을 확인할 수 있다.<br>
![1](/assets/images/210119-1.png)<br>

## Test Code

JUnit5로 테스트 코드를 작성해보자.<br>

```java
import com.cadi.team3.domain.Account;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.ResultActions;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@SpringBootTest
@AutoConfigureMockMvc
class AccountControllerTest {

    private Account createUser() {
        Account account = Account.builder()
                .nickname("testUser")
                .email("testUser@naver.com")
                .password("123456789")
                .role(Role.ROLE_USER)
                .emailVerified(false)
                .build();

        return account;
    }

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private ObjectMapper objectMapper;

    @MockBean
    private JavaMailSender javaMailSender;

    @Autowired
    AccountRepository accountRepository;

    @BeforeEach
    public void tearDown() {
        accountRepository.deleteAll();
    }


    @DisplayName("가입 Valid 확인 #1 - form 값이 잘 못 된 경우")
    @Test
    public void sign_up_form_test1() throws Exception {
        // given
        SignupDto signupDto = SignupDto.builder()
                .nickname("vividswan2131232131232131242145**")
                .password("1234")
                .email("vividswan")
                .build();

        // when
        final ResultActions perform = mockMvc.perform(post("/sign-up")
                .content(objectMapper.writeValueAsString(signupDto))
                .contentType(MediaType.APPLICATION_JSON));

        //then
        perform.andExpect(status().is4xxClientError());
    }

    @DisplayName("가입 Valid 확인 #2 - 올바른 form 값인 경우")
    @Test
    public void sign_up_form_test2() throws Exception {
        //given
        SignupDto signupDto = SignupDto.builder()
                .nickname("vividswan")
                .password("1234567890")
                .email("vividswan@naver.com")
                .build();

        //when
        final ResultActions perform = mockMvc.perform(post("/sign-up")
                .content(objectMapper.writeValueAsString(signupDto))
                .contentType(MediaType.APPLICATION_JSON));

        //then
        perform.andExpect(status().is2xxSuccessful());
    }
}
```

![2](/assets/images/210119-1.png)<br>
테스트 코드가 통과되었다.<br>