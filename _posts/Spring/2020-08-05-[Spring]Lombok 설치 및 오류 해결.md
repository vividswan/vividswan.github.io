---
title: "[Spring] Lombok 설치 및 오류 해결"
tags:
  - Spring
date: 2020-08-05T08:06:00-05:00
key: "[Spring] Lombok 설치 및 오류 해결"
---

## Lombok 설치 및 사용

@Getter, @Setter 등을 지원하여 코드 양을 줄여주는 Lombok을 Spring 프로젝트에 설치하고 오류를 해결해보자.

<!--more-->

### gradle.build에 의존성 추가

gradle.build의 dependencies에 Lombok에 대한 의존성을 추가해야 된다.

```java
dependencies {
	//...
    compile('org.projectlombok:lombok')
	//...
}
```

### Plugins에서 Lombok 설치

윈도우 기준으로 `ctrl + shift + a`를 누르면 Action으로 이동한다.<br>
Plugins를 검색해서 들어간 뒤, Lombok을 install 해주고 intellij 프로젝트를 재부팅한다.<br>

![1](/assets/images/200805-1.png)

### enable annotation processing 활성화

`setting -> Build, Execution, Deployment -> Annotation Processors` 로 들어간 뒤,<br>
`enable annotation processing` 를 체크해준다.<br>

![2](/assets/images/200805-2.png)

### gradle.build에 다시 추가

여기까지가 Lombok을 설치하기까지의 과정이지만, Lombok에 관련된 어노테이션을 사용 시<br>
`cannot find symbol` 라는 로그 메시지와 함께 실행이 되지 않았다.

build.gradle에 annotaition에 관한 의존성을 추가하니 해결되었다.<br>

```java
dependencies {
	//...
    compile('org.projectlombok:lombok')
    annotationProcessor('org.projectlombok:lombok')
	//...
}
```

의존성을 추가 후 Lombok과 관련된 어노테이션들이 정상적으로 실행되었다.<br>
