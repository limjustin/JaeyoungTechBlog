---
emoji: 🔐
title: 쿠키 vs 세션 vs 토큰 이해하기
date: '2022-10-26 17:54:00'
author: 키맨
tags: 개발 github-pages gatsby
categories: 개발
thumbnail: './thumbnail.png'
---

> 이번에는 개발자라면 많이 들어본 쿠키와 세션 그리고 토큰에 대하여 포스팅해보고자 합니다. 자주 등장하는 애들이지만 잘 알지 못하였기에 이번 기회에 정확한 의미와 활용까지 해보는 시간을 가지도록 하겠습니다.😁

## 🤔 왜 사용하는가?

많은 사람이 알듯이 인증 및 인가를 다룰 때 주로 사용한다. 그렇다면 왜 이 친구들이 필요한지 알아야 한다. 먼저 그전에 알아야 하는 HTTP 프로토콜이 있다.

### 📡 HTTP 프로토콜

HTTP 프로토콜은 서버/클라이언트 가 데이터를 주고받기 위해 따르는 프로토콜이다. 클라이언트가 요청(request)을 보내면 서버는 요청을 처리해서 응답(response)한다. 하지만 HTTP 프로토콜의 특징들 때문에 문제점이 생긴다. HTTP 프로토콜의 특징을 확인해보자

- Connectionless(비 연결지향) : 클라이언트가 서버에 요청한 후 응답받으면 연결을 바로 끊는 특징
- Stateless(상태 유지 없음) : 연결을 끊는 순간 클라이언트의 상태를 유지하지 않는 특징

이러한 특징들 때문에 큰 문제점이 생길 수 있다. 클라이언트의 입력 정보를 유지할 수 없기 때문에 발생할 수 있는 일은 너무나도 많다. `예를 들어 쿠팡에서 로그인한 다음 메인화면으로 왔다. 전에 담아 두었던 장바구니가 궁금하여 클릭하였는데 바로 앞에 로그인하였지만, 로그인하였지만 또다시 로그인해야 하는 치명적인 문제가 생긴다.` 이렇든 클라이언트의 요청 또는 입력정보를 유지할 필요가 있는 경우가 많은데 이런 필요한 정보를 저장해두고 활용할 수 있는 기능이 바로 쿠키, 세션, 토큰이다.

## 🍪 Cookie(쿠키)

웹사이트를 접속하다 보면 쿠키 허용하라는 문구를 한 번씩 본 적 있는 거 같다. `Cookie(쿠키)` 란 서버가 아닌 브라우저에 저장되는 작은 데이터이다. 브라우저는 클라이언트의 컴퓨터에 설치된 것이므로 `클라이언트가 가지고 있는 데이터`라고 생각하면 된다. 이러한 쿠키는 사용자 인증에도 사용될 수 있지만 우리가 자주 보는 `창 띄우지 않기`, `비회원 장바구니` 등 다양하게 사용될 수 있다.

다만 사용자가 가지고 있는 데이터이기 때문에 쉽게 수정, 삭제할 수 있고 당사자뿐만 아니라 제 3자가 조회하는 것도 가능하기 때문에 개인 정보를 담은 내용이나 중요한 데이터를 저장하지는 말아야 한다.

## 💾 Session(세션)

`Session(세션)`이란 쿠키와 다르게 `각각의 클라이언트 정보를 서버가 가지는 방식이다.` HTTP 프로토콜파트에서 언급했던 것처럼 우리는 사용자가 한번 로그인하면 더 이상 아이디와 비밀번호를 입력하지 않아도 되도록 해야 하는데 이를 해결하기 위해 사용하는 것이 바로 `세션`이다.

### 🏃🏻 동작방식

1. 클라이언트가 서버에 올바른 아이디와 비밀번호를 입력하여 로그인에 성공하면 서버는 `세션 아이디`라는 데이터를 만든다
2. 서버는 세션 아이디를 클라이언트에 전달하고, 메모리에 이 세션 아이디가 누구의 것인지 적어서 보관한다.
3. 클라이언트는 서버로부터 받은 `세션 아이디`를 쿠키에 저장한 다음 앞으로의 모든 요청에 `세션 아이디`를 함께 전달한다.
4. 서버는 클라이언트가 요청한 것을 받으면 `세션 아이디`가 적혀 있는지 확인하고, 있다면 서버 메모리에 보관되어 있던 세션 아이디 중에 동일한 정보가 있는지 파악하고 계정을 찾는다.

## 🪙 Token(토큰)

`Token(토큰)`이란 메모리 공간을 많이 차지하는 `Session(세션)`의 대안으로 사용자에게 `세션 아이디` 대신 `Token(토큰)`을 발급해주는 것이다. 주로 `JWT`를 많이 사용하는데 `JWT`는 인증에 필요한 정보들을 암호화한 토큰을 의미한다.

### 🏃🏻 동작방식

1. 클라이언트가 서버에 올바른 아이디와 비밀번호를 입력하여 로그인에 성공하면 서버는 `JWT` 생성하고 클라이언트에게 전송한다.
2. 클라이언트는 서버로부터 받은 `JWT`을 쿠키에 저장한 다음 필요할 때마다 `JWT`를 서버에 전달한다.
3. 서버는 `디비에 저장된 것을 비교하는 것 없이` 자기가 발급한 토큰임을 알아보고 클라인언트의 요청을 허용한다.

## 🤔 뭐가 더 좋을까?

### 🏡 확장성

사실 이 부분이 토큰을 많이 사용하는 이유이기도 한 것 같다. 일반적으로 서버 확장 방식은 수직 확장이 아닌 수평 확장을 한다. (좋은 성능의 한대가 아니라 여러 대의 서버가 요청을 처리한다) 세션 방식은 여러 대의 서버가 세션 불일치가 생기지 않도록 해줘야 하는데 이 작업이 꽤 어려워 보인다. 하지만, 토큰 방식은 클라이언트가 정보를 저장하기 때문에 세션의 문제점에서 벗어난다.

### 👷🏼 안전성

세션의 경우 모든 정보를 서버에서 관리하기 때문에 보안 측면에서 더 유리하다. 하지만 토큰의 경우에는 클라이언트가 모든 인증정보를 가지고 있기 때문에 토큰을 탈취당하면 만료되기 전까지 피해를 볼 수 있다.

## 👍🏻 대안

토큰 방식 각자의 단점을 보완하고자 하나의 대안이 기존 JWT 토큰 방식에 `refresh token`을 도입하는 것이다.

### 🏃🏻 동작방식

1. 클라이언트가 서버에 올바른 아이디와 비밀번호를 입력하여 로그인에 성공하면 `Access Token` 과 `Refresh Token`을 발급한다. 이때, `Access Token`은 유효 기간을 짧게, `Refresh Token`은 유효 기간을 길게 설정한다.
2. 서버는 `Access Token` 과 `Refresh Token`을 클라이언트에 전달하고, 메모리에 `Refresh Token`의 상응 값을 저장한다.
3. 클라이언트는 `Access Token`이 유효하다면 `Access Token`을 유효하지 않다면 `Refresh Token`을 보내어 서버 측에 `Access Token`을 다시 발급받는다.

이러한 방식은 `Access Token`이 탈취되더라도 짧은 유효기간이 지나면 사용할 수 없기 때문에 보안 측면에서 좋아지고 정상적인 클라이언트는 유효기간이 지나더라도 `Refresh Token`을 사용하여 재발급받을 수 있다.

### 😭 Refresh Token이 탈취된다면

사실 공부를 하며 이 부분에 대하여 생각하게 되었고 찾아본 결과 다양한 방법들이 있지만 결국엔 Refresh Token은 탈취당하지 않도록 해야 한다는 것으로 생각됩니다. 만약 `Access Token`, `Refresh Token` 둘 다 탈취된다면? (후... 어렵네요) 좋은 해결법을 알고 있으신 분들은 댓글로 남겨주시면 감사하겠습니다.

## 🧑🏻‍💻 소감

<br/>

**위 과정을 따라하시면서 궁금하신 점이 있다면 아래 `댓글`로 남겨주세요!👇**

```toc

```