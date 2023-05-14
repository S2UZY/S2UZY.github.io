---
title: "[Boostcourse] Web개발의 이해 (HTTP프로토콜, FE/BE)"
excerpt: "웹의 동작 방식, 요청데이터 포멧"

categories:
  - Web
tags:
  - [Web, Boostcourse]

permalink: /blog/web/boostcourse2

toc: true
toc_sticky: true

date: 2022-06-11
last_modified_at: 2022-06-11
---

## 웹 프로그래밍을 위한 프로그램 언어들

<mark>저급 언어</mark>

- 기계 중심의 언어
- 어셈블리어 ( Assembly Language )

<mark>고급 언어</mark>

- 사람 중심의 언어
- Java , Python, C/C++, Swift , C# 등등

<br>

## 웹의 동작 ( HTTP 프로토콜 이해 )

### 인터넷 (네트웍 통신)의 이해

- 인터넷 ≠ WWW(World Wide Web)
- 인터넷 기반의 대표 서비스중 하나  
  ![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse2/Untitled.png)

### 인터넷

- TCP/IP 기반의 네트워크가 전세계적으로 확대되어 하나의 연결된 네트워크들의 네트워크 = 수많은 네트워크들의 결합체

### HTTP란?

- 팀 버너스리와 CERN에서 웹 브라우저와 HTML, HTTP 관련 기술을 발명
- 서버와 클라이언트가 인터넷상에서 데이터를 주고 받기 위한 프로토콜
- 어떤 종류의 데이터도 전송할 수 있도록 설계되어있다.
- 부스트코스에선 http v1.1 사용

### HTTP 작동방식

![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse2/Untitled%201.png)

HTTP는 서버/클라이언트 모델을 따른다. 클라이언트가 먼저 요청 → 서버가 응답하는 것이다. HTTP는 무상태 프로토콜이라고도 이야기한다. statless 무상태 방식이란 HTTP는 서버가 응답을 하고나면 클라이언트와 연결 끊음 → 다시 요청해도 이전 클라이언트인지 모르는 것을 말한다.

<mark>무상태 방식 장점</mark>

- 불특정 다수를 대상으로 하는 서비스에 적합
- 계속 연결상태를 유지하는 것이 아니라서 훨씬 많은 요청과 응답 가능

<mark>무상태 방식 단점</mark>

- 특정 정보를 유지하기 힘들수도 있다. 그래서 Cookie와 같은 기술이 등장

<br>

## HTTP 요청/응답 데이터 포멧

### 요청 데이터 포멧

<Mark>Header</Mark>  
첫번째 줄:

- 요청 메서드 (GET, POST, PUT,DELETE,HEAD, OPTIONS, TRACE 등 )
- 요청 URL ( /test/query?a=10 )
- 프로토콜 버전 명시 ( HTTP/1.1 )

두번째 줄부터:

- 여러줄의 헤더 정보
- 헤더 명과 헤더값이 콜론으로 구분
- 각 줄은 라인피드와 캐리지리턴으로 구분

<Mark>Body</Mark>

- get 방식은 요청바디가 없다. url에 붙여서 갖고 간다.
- POST, PUT 사용됐을때 들어온다.

### 응답 데이터 포맷

<Mark>Header</Mark>  
첫번째 줄 :

- 응답 HTTP 프로토콜의 버전 ( HTTP/1.1)
- 응답코드 (200)
- 응답메시지 (ok)

두번째 줄부터:

- 날짜, 웹 서버, 이름과 버전, 콘텐츠 타입, 캐시 제어방식 , 콘텐츠 길이등

<Mark>Body</Mark>

- 실제 응답 리소스 데이터
- URL (Uniform Resource Locator)
- 인터넷 상의 자원의 위치
- 특정 웹 서버의 특정파일에 접근하기 위한 경로 혹은 주소

![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse2/Untitled%202.png)

- 물리적인 서버를 찾기 위해서는 ip주소나 도메인 주소 필요
- 물리적인 컴퓨터를 찾은 후에 해당 컴퓨터안에 등장하는 소프트웨어 서버를 찾기 위해서는 포트 값 필요
- 각 서버는 하나의 방이 필요하다. 포트번호가 각각 달라야한다.
- HTTP 서버는 기본 포트값이 80번

<br>

---

## 웹 프론트엔드

- 사용자에게 웹을 통해 다양한 콘텐츠(문서, 동영상, 사진 등)을 제공한다.
- 또한 사용자의 요청에 의해 반응해서 동작한다.

## 웹 백엔드

- 서버입장에서 개발진행 서버 사이드라 말한다.
- 백엔드가 알아야 할 것
  - 웹의 동작원리
  - 알고리즘, 자료구조 등 프로그래밍 기반 지식
  - 운영체제, 네트워크 등에 대한 이해
  - 프레임워크에 대한 이해
  - DBMS에 대한 이해와 사용방법
