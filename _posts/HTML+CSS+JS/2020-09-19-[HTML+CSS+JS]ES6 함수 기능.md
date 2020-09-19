---
layout: article
title: "[HTML/CSS/JS] ES6 함수 기능"
tags:
  - HTML/CSS/JS
date: 2020-09-19T08:06:00-05:00
key: "[HTML/CSS/JS] ES6 함수 기능"
---

# ES6의 함수

JavaScript ES6의 함수 기능과 추가된 것을 알아본다.

<!--more-->

## 타입 선언이 지원 x

```javascript
Function log(message){
	console.log(message);
}
```

위와 같이 메시지를 출력하는 함수를 만든다고 하자.<br>
자바스크립트는 함수를 선언할 때 인자의 타입 선언이 지원되지 않는다.<br>
그러므로, 위의 `message`라는 파라미터를 만들 때 함수의 인터페이스만 봐선 개발자가 어떤 타입을 원하고 선언했는지 알 수 없다.<br>
이러한 타입 선언은 타입 스크립트로 넘어가야 지원된다.<br>


## Default Parameters


ES6로 넘어오며 파라미터에 초깃값을 줄 수 있는 Default Parameters가 추가되었다.<br>

```javascript
Function testFunc(param1, param2){
  console.log(`${param1} ${param2}`);
}
```

위처럼 두 개의 인자를 출력하는 함수가 있을 때, 파라미터의 값이 없다면 조건문 등을 걸어서 따로 처리해 줬어야 했다.<br>
하지만 디폴트 파라미터스의 기능을 사용하면 초깃값을 미리 선언할 수 있다.<br>

```javascript
Function testFunc(param1, param2 = "Default Setting"){
  console.log(`${param1} ${param2}`);
}
```
위처럼 선언해 주면, `param2`의 값이 들어오지 않는다면 디폴트 값이 들어가게 된다.<br>

## Rest Parameters

`Rest Parameters` 역시 ES6에 추가된 기능이다.<br>
함수의 인자를 여러 개로 받고 싶을 때, `... 인자 이름`으로 선언하면 배열을 여러 개 받을 수 있다.<br>

```javascript
Function printAll(…args){
	for(const arg of args){
		console.log(arg);
	}
}
```

위 식과 같이 `... args`로 받게 된다면, args로 여러 개의 인자를 받을 수 있고, for 문을 통해 순회할 수 있다.<br>

## Arrow Function

기존의 함수 선언에 대한 코드 양을 줄여주는 Arrow Function이 있다.<br>

```javascript
Function testFunc(param1, param2){
  console.log(`${param1} ${param2}`);
}
```

위에서 예시로 들었던 함수를 Arrow Function으로 수정하자.<br>

```javascript
Const testFunc = (param1, param2) => console.log(`${param1} ${param2}`);
```

위와 같은 형식으로 코드 양을 줄여서 표현할 수 있다.<br>
함수 안에 여러 줄의 내용이 블록 안에 들어가 있다면 return 표시가 필요하다.<br>