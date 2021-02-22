---
layout: article
title: "[AWS+배포] EC2 접속 설정 (MAC OS)"
tags:
  - AWS+배포
date: 2020-10-16T08:06:00-05:00
key: "[AWS+배포] EC2 접속 설정 (MAC OS)"
---

# EC2 접속 설정

EC2에 편하게 접속할 수 있도록 설정을 해보자.<br>

<!--more-->

## 생성된 인스턴스 + 탄력적 ip

이 방법을 실행하기 위해선 인스턴스가 생성되어 있어야 하고, 탄력적 ip를 등록해야 한다.<br>
탄력적 ip가 없다면 생성한 뒤 인스턴스와 연결해 주자.<br>
**탄력적 ip는 생성한 뒤 인스턴스와 연결을 안 해놓으면 요금이 부과되므로 바로 인스턴스와 연결해 주도록 하자!!**<br>
![1](/assets/images/201016-1.png)<br>

## 키 패어 파일 이동

terminal에서 `cp` 명령어를 이용해 키 페어를 `~/.ssh/`로 이동해 주자.<br>

> cp "키 페어 저장 위치" ~/.ssh/

![2](/assets/images/201016-2.png)<br>

## pem 권한 변경

`chmod` 명령어를 이용하여 pem의 권한을 변경해 주자.<br>

> chmod 600 ~/.ssh/"pem 파일 이름"

![3](/assets/images/201016-3.png)<br>

## config 파일 생성

pem 키를 옮긴 위치에 config 파일을 생성해야 한다.<br>

> vim ~/.ssh/config

입력 모드인 i를 누르고 config 파일을 다음과 같은 식으로 작성한다.<br>


```javascript
Host "서비스 이름"
    HostName "탄력적 ip"
    User ec2-user
    IdentityFile ~/.ssh/"pem 파일 이름"
```

![4](/assets/images/201016-4.png)<br>

esc로 입력 모드를 나간 뒤, wq를 입력하여 저장해 주고 종료한다.<br>

종료 뒤에 config 파일의 권한을 수정해 주자.<br>

> chmod 700 ~/.ssh/config

## 접속

이제 ssh 뒤에 등록한 서비스 명을 입력하면 편하게 접속할 수 있다.<br>

![5](/assets/images/201016-5.png)<br>