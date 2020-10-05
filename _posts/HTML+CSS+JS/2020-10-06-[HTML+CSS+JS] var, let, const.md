---
layout: article
title: "[HTML/CSS/JS] var, let, const"
tags:
  - HTML/CSS/JS
date: 2020-10-06T08:06:00-05:00
key: "[HTML/CSS/JS] var, let, const"
---
# var, let, const

자바스크립트에서 변수를 선언할 때 사용하는 `var, let, const`의 차이를 알아보자.

<!--more-->

## var

var는 const와 다르게 값이 변할 수 있는 변수를 선언할 때 사용된다.<br>
const와의 차이는 변수이냐 상수이냐인데 let 과의 차이는 무엇일까?<br>
var는 function scope이고, let과 const는 block scope이다.<br>

```javascript
  if(true){
      var num = 100;
  }
  console.log(num); // 100 출력
  num += 200;
  console.log(num); // 300 출력
```

위의 예제를 확인해보자.<br>
블록 안에 num이 선언되고 블록이 끝났기 때문에 num이 소멸될 것 같지만 var는 function scope이기 때문에 값을 출력한다.<br>

```javascript
  function testFunc(){
      var num = 100;
      console.log("testFunction");
      return;
  }
  testFunc();
  console.log(num);
```

위의 예제가 function scope를 벗어나서 num을 호출하므로 `num is not defined` 메시지가 나온다.<br>

## let

```javascript
  if(true){
      let num = 100;
  }
  console.log(num); // 오류
```

let은 block scope이므로 블록을 벗어나면 출력되지 않고 오류가 난다.<br>

```javascript
  let num = 100;
  console.log(num); // 100 출력
  num = 200;
  console.log(num); // 200 출력
```

블록 범위에만 있다면 let은 변수 선언에 사용되기 때문에 값이 얼마든 바뀔 수 있다.<br>
var는 block을 벗어나도 사용되므로 여러 혼란을 줄 수 있기 때문에 지양하고 let을 사용하는 것이 좋다.<br>

## const

const 또한 block scope이므로 block이 끝나면 소멸된다.<br>

```javascript
  if(true){
      const num = 100;
  }
  console.log(num); // 오류
```

또 const는 상수를 선언할 때 사용하므로, 값을 변경하면 오류를 출력한다.<br>

```javascript
  const num = 100;
  console.log(num); // 100 출력
  num = 200;
  console.log(num); // 오류
```
`TypeError: Assignment to constant variable.` 오류가 출력된다.<br>