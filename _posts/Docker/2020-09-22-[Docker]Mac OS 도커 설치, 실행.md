---
title: "[Docker] Mac OS 도커 설치, 실행"
tags:
  - Docker
date: 2020-09-22T08:06:00-05:00
key: "[Docker] Mac OS 도커 설치, 실행"
---

# 도커 실행 & run

Mac OS에 도커를 설치하고 hello-world 이미지를 run 해보자.<br>

<!--more-->

## 도커 홈페이지

[https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)에 접속한다.<br>
![1](/assets/images/200922-1.png)<br>
Docker Desktop을 설치해야 하고, `Download for Mac (stable)`을 클릭하여 설치 프로그램을 다운로드한다.<br>

## Docker.dmg

`Docker.dmg` 파일을 실행하면 다음과 같은 그림이 나온다.<br>
![2](/assets/images/200922-2.png)<br>
도커를 어플리케이션으로 드래그해주면 설치가 완료되고 상단에 도커 아이콘이 추가되있는 것을 알 수 있다.<br>

## 회원가입

다운로드 과정에선 회원일 필요가 없지만, 도커를 사용할 땐 회원으로 로그인이 되어있어야 한다.<br>
회원가입을 하도록 하자.<br>

## 실행

도커에 회원 로그인이 되어있어야 하고, 최상단에 초록불이 들어왔을 때 실행이 가능하다.<br>
![3](/assets/images/200922-3.png)<br>
위 조건들이 만족되어 있다면 터미널로 간단한 실행을 해보도록 하자.<br>
터미널을 켜고 `docker run hello-world` 명령어를 입력해 주자.<br>
![4](/assets/images/200922-4.png)<br>

> Unable to find image<br>

'hello-world:latest' locally<br>
처음에 hello-world라는 이미지가 로컬의 캐시 저장소에 없기 때문에 이런 메시지가 뜬다.<br>
로컬에 없는 이미지이기 때문에 도커 서버가 도커 허브에서 해당 이미지를 가져와준다.<br>
로딩 이후 도커에 대해 기본적인 내용을 알려주는 hello-world 이미지가 실행된다.<br>

![5](/assets/images/200922-5.png)<br>
