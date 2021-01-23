---
title: "[Spring] Swagger2 적용"
tags:
  - Spring
date: 2021-01-23T08:06:00-05:00
key: "[Spring] Swagger2 적용"
---

# Swagger2 적용

API 문서 자동화를 해주는 Swagger2를 스프링 프로젝트에 적용해보자.

<!--more-->

## 의존성 추가

gradle을 이용한다면 build.gradle에 다음과 같은 의존성을 추가해 준다.<br>

```gradle
    compile group: 'io.springfox', name: 'springfox-swagger2', version: '2.9.2'
    compile group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.9.2'
```

## Swagger2Config

Config 패키지를 만들고 다음과 같은 Config 클래스를 만들어준다.<br>

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@EnableSwagger2
@Configuration
public class Swagger2Config {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.cadi.team3"))
                .paths(PathSelectors.ant("/api/**"))
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("cadi project")
                .version("1.0")
                .description("movie & sns project apis")
                .build();
    }
}
```
config에 등록된 Docket Bean으로 Swagger가 적용될 전체 경로를 설정해 준다.<br>
base가 되는 package와 Swagger2로 확인할 api의 요청 경로를 설정해 줄 수 있고, 바로 밑에 선언한 apiInfo 메서드를 apiInfo로 지정할 수 있다.<br>
apiInfo()는 ApiInfoBuilder()를 이용해 직관적으로 api 문서에 대한 정보를 설정할 수 있다.<br>
문서의 이름, 버전, 설명 등을 설정하고 build() 해준다.<br>

## Controller에서 api 부가 정보 설정

다음과 같이 swagger-ui에 등록할 api 설명을 설정할 수 있다.<br>

```java
@RequiredArgsConstructor
@RestController
@RequestMapping("/api")
public class AccountController {

    private final AccountService accountService;

    @PostMapping("/sign-up")
    @ApiOperation(value = "회원가입", notes = "signupDto로 전송")
    public ResponseEntity<?> singUpRequest(@RequestBody @Valid SignupDto signupDto, Errors errors){
        return accountService.signUp(signupDto, errors);
    }

    @GetMapping("/get-account")
    @ApiOperation(value = "로그인 회원정보 조회")
    public ResponseEntity<?> getAccount(@AuthenticationPrincipal AccountPrincipal accountPrincipal){
        return new ResponseEntity<>(accountPrincipal, HttpStatus.OK);
    }

}
```

> @ApiOperation(value = "회원가입", notes = "signupDto로 전송")
> `@ApiOperation`을 이용해 value 값과 notes로 부가 설명을 등록할 수 있다.<br>

## Spring Security 설정

스프링 시큐리티와 Swagger2를 함께 사용한다면 Security의 보안에 의해 swagger2의 ui가 작동 안 하는 경우가 있다.<br>
이를 위해 `WebSecurityConfigurerAdapter`를 상속한 SecurityConfig에서 다음과 같이 오버라이딩 해준다.<br>

```java
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/v2/api-docs/**");
        web.ignoring().antMatchers("/swagger.json");
        web.ignoring().antMatchers("/swagger-ui.html");
        web.ignoring().antMatchers("/swagger-resources/**");
        web.ignoring().antMatchers("/webjars/**");
    }
```

## Swagger 확인

baseUrl 뒤에 "/swagger-ui.html"을 붙여주고 웹에서 접속해보자.<br>

![1](/assets/images/210123-1.png)<br>

화면에 나오는 api를 swagger2를 통해 점검하고 확인해 볼 수 있다.<br>