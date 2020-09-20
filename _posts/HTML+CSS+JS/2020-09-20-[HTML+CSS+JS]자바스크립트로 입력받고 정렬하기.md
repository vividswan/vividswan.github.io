---
layout: article
title: "[HTML/CSS/JS] 자바스크립트로 입력받고 정렬하기"
tags:
  - HTML/CSS/JS
date: 2020-09-20T08:06:00-05:00
key: "[HTML/CSS/JS] 자바스크립트로 입력받고 정렬하기"
---
# 자바스크립트로 입력받고 정렬하기

사용자에게 입력 값을 받아 split()으로 배열로 만들고 sort 함수를 이용해 배열해보자.<br>

<!--more-->

## prompt()

자바스크립트를 이용해 웹에서 사용자에게 정보를 입력받을 땐, prompt()를 이용한다.<br>
웹에서 prompt()가 동작하면 웹상에 알림 창이 뜨고 사용자에게 값을 입력받을 수 있다.<br>
알림 창에 들어갈 내용은 prompt`("들어갈 내용")`과 같은 형식으로 넣을 수 있다.<br>

## split(), join()

사용자에게 문자열을 입력받은 변수에 split() 함수를 이용한다면, split() 안에 지정한 문자열을 기준으로 잘라내어 배열을 만들어준다.<br>
만들어진 배열에 join() 함수를 이용하면, join() 안에 지정한 문자열을 기준으로 배열을 합쳐서 하나의 문자열로 바꿔준다.<br>
즉 split()과 join()은 서로 반대의 일을 하기 때문에, <br>

```javascript
  let str = "1 2 3 4 5";
  str.split(" ");
```

위와 같은 식에서는 str이 1,2,3,4,5 를 가지고 있는 배열로 바뀌지만 다시 `join(" ")`을 해준다면 문자열 `"1 2 3 4 5"`로 전환된다.<br>

## sort()

자바스크립트의 sort()는 파라미터로 함수를 넣어주고 그 함수의 return 값으로 오름차순, 내림차순이 정해진다.<br>

```javascript
sort(function(a,b){
    return a-b;
});

sort(function(a,b){
    return b-a;
});
```
위와 같은 함수를 인자로 가진 sort는 오름차순이 되고, 아래와 같은 경우엔 내림차순이 된다.<br>

sort 내부 함수의 인자와 리턴 값을 보면, <br>
> 1번 인자가 2번 인수보다 작을 경우 - 값<br>
> 두 인자가 같을 경우 0<br>
> 1번 인자가 2번 인자보다 클 경우 + 값<br>
가 되고 이를 바탕으로 정렬을 해준다.<br>


## 구현

사용자에게 여러 개의 숫자를 입력받고, 그 숫자들을 오름차순으로 정렬한 결과를 출력해보자.<br>

```javascript
let strArr = prompt("숫자를 입력해 주세요").split(" ");
let ascStrArr = strArr.sort(function(a, b){
return a-b;
});
console.log("오름차순 -> "+ascStrArr);
```

![1](/assets/images/200920-1.png)<br>
prompt()로 알림 창이 나오고 값을 입력할 수 있다.<br>

![2](/assets/images/200920-2.png)<br>
오름차순으로 정렬된 결과를 확인할 수 있다.<br>