---
title: "[Spring] OAuth 2.0 동작 과정"
tags:
  - Spring
date: 2020-09-04T08:06:00-05:00
key: "[Spring] OAuth 2.0 동작 과정"
---

# OAuth 2.0 동작 과정

<!--more-->

## OAuth의 필요성

한 명의 회원이 N 개의 사이트에 회원 가입을 한다면, 인터넷 사용자의 N 배수만큼의 아이디가 생긴다.<br>
이는 그만큼 개인 정보가 많이 사용된다는 의미이고, 저장 공간의 수요와 개인 정보에 대한 보안 요소가 더 많아진다는 것을 의미한다.<br>
이때 OAuth를 사용하여 네이버와 카카오 같은 사이트로 로그인을 맡기면 맡긴 홈페이지는 인증처리에 대한 수고(회원가입, 회원 탈퇴, 기간에 따른 관리 등등)을 덜 수 있다.<br>

## OAuth의 두 단계
OAuth는 크게 두 단계로 나뉜다.<br>
> - 콜백으로 코드값을 받는 과정
> - Access Token으로 데이터 요청의 권한을 받는 과정

첫 번째 과정은 User가 OAuth로 인증해 주는 기관(네이버, 카카오 등)에 인증을 받아서 그 인증을 내 홈페이지로 콜백 하는 과정이다.<br>
두 번째 과정은 그 콜백 코드를 이용하여 User의 정보를 내 홈페이지가 요청하여 User 정보를 받을 수 있게 하는 과정이다.<br>

## 기존의 로그인 과정

User가 내가 만든 홈페이지에 로그인을 한다고 생각하자.<br>
그렇다면 User가 클라이언트가 되고 내가 만든 홈페이지가 서버가 된다.<br>
클라이언트인 User가 로그인에 대한 요청을 해주면 서버인 홈페이지가 로그인 응답을 해주는 과정이다.<br>

## OAuth 로그인 과정 - 구성

![1](/assets/images/200904-1.png)

OAuth의 로그인을 알아보기 전에 구성을 살펴봐야 한다.<br>
기존의 로그인과는 다르게 아래와 같이 이루어진다.<br>

- 유저 -> 리소스 오너(리소스 : 개인 정보)
- 내 홈페이지 -> 클라이언트(API 서버의 클라이언트)
- 네이버, 카카오 등의 API 서버(인증을 해주는 서버)
- 지원 서버 -> 리소스 서버(리소스 : 개인 정보)

## OAuth 로그인 과정 - 1. 콜백 과정

User가 다이렉트로 OAuth를 처리해 주는 API 서버에 로그인 요청을 보낸다.<br>
로그인이 확인되고, API 서버에 대한 정상적인 접근이 이루어진다면 내 홈페이지로 리다이렉트(콜백) 후 코드 값을 전달해 준다.<br>
내 홈페이지에선 코드값이 정상적인지 확인하고 인증받았다는 것을 알게 된다.<br>
이렇게 하면 첫 번째 과정인 인증처리가 완료되었다.<br>

## OAuth 로그인 과정 - 2. Access Token

내 홈페이지는 코드 값을 API에 보낸 뒤 데이터를 조회할 수 있는 권한을 요청한다.<br>
API 서버는 코드를 조회하고 값이 정상이라면 `Access Token`을 전해준다.<br>
내 홈페이지는 `Access Token`으로 권한을 부여받았으므로, User에 정보에 접근할 수 있는 권한을 얻는 것이다.<br>

이와 같이 OAuth는 카카오나 네이버 같은 사이트에 개인 정보를 가지고 있는 유저(리소스 오너)가 내 홈페이지에게 카카오나 네이버 같은 사이트의 정보를 조회할 수 있는 권한을 위임해 주는 과정이라고 할 수 있다.<br>

## 스프링 공식 OAuth

스프링 공식 OAuth 주체는 facebook, google이며 OAtuh Client 라이브러리가 지원해 주어서 더 편하게 연동할 수 있다.<br>
위의 다른 국내와 해외의 여러 사이트들도 따로 연동하는 방법이 존재한다.<br>