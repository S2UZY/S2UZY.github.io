---
title:  "[Boostcourse] Browser"
excerpt: "브라우저의 기본구조, 렌더링엔진, DOM트리, 렌더트리"

categories:
  - Web
tags:
  - [Web, Boostcourse]

permalink: /web/boostcourse3/

toc: true
toc_sticky: true
 
date: 2022-06-11
last_modified_at: 2022-06-11
---

## 브라우저 표준
웹 브라우저는 HTML과 CSS명세에 따라 HTML파일을 해석하고 표시한다. 명세는 웹 표준화 기구인 W3C (World Wide Web Consortium)에서 정한다 → 호환성 좋아짐

<br>

## 브라우저 기본 구조
![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled.png)
1. <mark>User Interface </mark>
    - 주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등 요청한 페이지를 제외한 나머지 부분
2. <mark>Browser engine</mark>
    - 사용자 인터페이스와 렌더링 엔진 사이의 동작 제어
    - 브라우저마다 브라우저 엔진 가지고 있다.
3. <mark>Rendering engine</mark>
    - 요청한 콘텐츠를 표시. HTML과 CSS를 파싱하여 화면에 표시
4. <mark>Networking</mark>
    - HTTP 요청과 같은 네트워크 호출에 사용됨
5. <mark>Java interpreter</mark>
    - 자바스크립트 코드를 해석하고 실행
6. <mark>UI Backend</mark>
    - 콤보박스와 창 같은 기본적인 장치를 그림. 플랫폼에서 명시하지 않은 일반적인 인터페이스
    - OS 사용자 인터페이스 체계를 사용
7. <mark>Data Persistence</mark>
    - 자료를 저장하는 계층
    - 쿠키와 저장하는 것과 같이 모든 종류의 자원을 하드 디스크에 저장

크롬은 대부분의 브라우저와 달리 탭마다 별도의 렌더링 엔진 인스턴스를 유지한다고 한다. 

탭마다 독립된 프로세스로 처리된다.

<br>

## 렌더링 엔진
### 렌더링 엔진의 역할
- 요청 받은 내용을 브라우저 화면에 표시
- HTML 및 XML 문서와 이미지 표시 가능

이 글에서 다루는 브라우저인 파이어폭스, 크롬, 사파리는 두 종류의 렌더링 엔진으로 제작

파이어폭스 : 게코(Gecko) 엔진

사파리, 크롬 : 웹킷 (Webkit) 엔진

<br>

## 동작 과정
렌더링 엔진은 통신으로부터 요청한 문서의 내용을 얻는 것으로 시작한다. 문서의 내용은 보통 8KB단위로 전송한다. 

다음은 렌더링 엔진의 기본적인 동작과정이다.
![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled%201.png)

*파싱 : 문자단위로 하나하나 해석해서 이 내용이 가직 의미들을 파악

1. DOM 트리 구축을 위한 HTML 파싱
    - 태그를 DOM 노드로 변환
    - 트리형태로 데이터들을 가지고 있게 된다.
2. 렌더 트리 구축
    - CSS 스타일 정보 파싱
    - 스타일정보와 + HTML표시 규칙
    - 렌더트리는 색상과 면적과 같은 시각적 속성이 있는 사각형 포함 → 화면에 순서대로 표시
3. 렌더트리 배치
    - 각 노드가 화면의 정확한 위치에 표시되는 것
4. 렌더트리 그리기
    - UI 백엔드에서 렌더트리의 각 노드를 가로지르며 형상을 만들어는 과정
  
  <br>

일련의 과정들이 점진적으로 진행된다. 좀 더 나은 사용자 경험을 위해 빠르게 내용을 표시하는데 <mark>모든 HTML을 파싱할때까지 기다리지 않고 배치와 그리기 과정 시작</mark>한다. 네트워크로부터 나머지 내용이 전송되기를 기다리는 동시에 받은 내용의 일부를 먼저 화면에 표시하는 것이다.

![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled%202.png)
렌더링 엔진마다 다르지만 위에 그림은 웹킷의 동작과정이다.

<br>

## 파싱
![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled%203.png)

**문서파싱**
- 브러우저가 코드를 이해하고 사용할 수 있는 구조로 변환하는 것
- 파싱 결과 구조를 파싱 트리 (parse tree) , 문법트리(syntax tree) 라고 부른다.
- 위에 사진은 2+3-1 같은 표현식을 파싱한 트리노드이다.


<br>

## DOM (Document Object Model)
DOM은 문서 객체 모델의 준말이다. DOM은 HTML 문서의 객체표현이고 자바스크립트, CSS같은 언어들이 DOM구조에 접근하여 커스텀 할 수 있게 연결 부분 역할을 갖는다. 

HTML에만 국한된 것이 아니라 XML 역시 DOM으로 표현해서 사용할 수 있다. DOM도 W3C에 의해 명세가 정해져있다. 

![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled%204.png)

DOM은 마크업과 1:1의 관계를 맺는 것을 위에 그림을 보고 알 수 있다.

<br>

## 웹킷 CSS 파서
![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled%205.png)

CSS파일은 스타일시트 객체로 파싱되고 각 객체는 CSS규칙을 포함한다.

이미지를 보면 왼쪽트리에 선택자가 들어가고 오른쪽 트리엔 선언문이 들어간다.

<br>

## DOM트리와 렌더트리의 관계
![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled%206.png)

위 그림처럼 렌더트리가 구성된다 할 수 있다.

블럭은 어떤 영역을 가지는 벽돌과 같은 것이라고 생각하면 되는데 얘는 블록이야~ 이런식으로 디스플레이 정보를 표현해주는 것이다.

<br>

## CSS Box model
![Untitled](/assets/images/posts_img/2022-06-11-web-boostcourse3/Untitled%207.png)

CSS 박스모델은 문서 트리에 있는 요소를 위해 생성되고 시각적 서식 모델에 따라 배치된 사각형 박스를 설명한다. 박스엔 content, padding, border, margin이 있다. 

각 노드는 이런 상자를 0개에서 n개 생성한다.

<br>
---
**Reference**

[NAVER D2](https://d2.naver.com/helloworld/59361)

[How Browsers Work: Behind the scenes of modern web browsers - HTML5 Rocks](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)