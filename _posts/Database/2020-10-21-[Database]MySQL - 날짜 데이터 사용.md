---
title: "[Database] MySQL - 날짜 데이터 사용"
tags:
  - Database
date: 2020-10-21T08:06:00-05:00
key: "[Database] MySQL - 날짜 데이터 사용"
---

# MySQL - 날짜 데이터 사용

mysql에서 날짜 데이터를 사용해보자.<br>
<!--more-->

## DATE_FORMAT

MySQL에서는 `DATE_FORMAT` 함수를 이용하여 날짜를 원하는 포맷으로 표현할 수 있다.<br>
함수의 인자를 다음과 같이 넣어서 사용할 수 있다.<br>

> DATE_FORMAT(NOW(), `'지정하고 싶은 형식'`); <br>

예를 들어<br>
```sql
`SELECT DATE_FORMAT(NOW(), '%y %m %d %a') as nowFormatting;`
```
다음과 같이 SELECT를 입력해보자.<br>

![1](/assets/images/201021-1.png)<br>

위와 같은 결과가 출력된다.<br>

## 지원되는 포맷 형식

DATE_FORMATE에서 두 번째 인자로 들어가는 포맷은 다음과 같다.<br>
오늘 날짜 `2020/10/21(수) 오후 10:20 30초`를 기준으로 하자.<br>

- 연도 : %Y(`2020`), %y(`20`)
- 월 : %M(`October`), %b(`Oct`), %m(`10`, 한자리 수이면 앞자리에 0을 포함해서 출력), %c (`10`)
- 일 : %D(`21st`), %d(`21`, 한자리 수이면 앞자리에 0을 포함해서 출력), e(`21`)
- 요일 : %W(`Wednesday`), %a(`Wed`), %w(`3`)
- 시 : %H(`22`), %k(`22`), %h(`10`), %l(`10`)
- 분 : %i(`10`)
- 초 : %S(`30`), %s(`30`)
- AM, PM : %p(`PM`)

## 출력

앞에 `%`를 붙여서 원하는 문자열과 조합해서 출력할 수 있다.<br>

MySQL에 다음과 같은 입력을 해보자.<br>
> SELECT DATE_FORMAT(NOW(), '오늘은 %Y년 %c월 %e일 %W이며, 현재 시간은 %p %l시 %i분 %s초입니다!') AS nowFormatting;

![2](/assets/images/201021-2.png)<br>

이런 방식의 출력이 가능하다.<br>