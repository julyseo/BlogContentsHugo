---
title: "[Architecture] IoC 관련 끄적 (IoC, DI, DIP, IoC Container)"
date: 2019-05-28
categories:
  - Architecture
  - DI/IoC
tags:
  - IoC
  - DI
  - DIP
  - IoC Container
  - Swift
  - 의존성 주입
thumbnailImagePosition: top
thumbnailImage: /res/img/ioc/ioc_diagram.jpg
# metaAlignment: center
---
IoC 관련 스터디를 하던 도중에 최신 자료의 부재, 모호한 설명이 많고,   
한 게시글에 이해하기 쉽게 만들어진 자료가 부족하다고 느껴져서   
내가 찾아보고 이해한 내용을 되새기는 차원에서 정리했다.
<!--more-->

<!--toc-->

# 들어가기에 앞서

인터넷에서 돌아다니는 글들을 모아 정리한 글이며, 내용이 틀릴 수 있음.  
예시 코드 내 클래스 명이라던가 메서드 명이라던가 아주 유명한 코드를 끌어다 썼지만,  
Swift 기반으로 직접 다시 작성하였다.  

> PS. 쓰고보니 이론적인건 크게 중요하지 않음.. 개념을 모르고도 쓰던 부분이 대부분

## 부족했던 것들
내가 구글링한 자료들에는 다음과 같은 내용이 부족했다는 느낌이 있었다.  

1. IoC, DI, DIP, IoC Container 를 한꺼번에 다루는 **한글 자료** 가 드물고 **최신 자료는** 더 드물다
2. IoC == IoC Container? - IoC 는 **개념** 인데 이걸 마치 IoC Container 로만 생각하여 적은 자료가 생각보다 많았음
3. IoC, DI, DIP, IoC Container 라는 네가지 요소가 왜 세트인 것 마냥 다뤄지는지, 각각의 연관 관계에 대한 설명이 미비함
4. 설명한답시고 마틴 파울러의 글을 번역만 해서 올린 글이 매우 많았음

그렇다. 찾다찾다 못찾아서 직접한 케이스다  
내가 댕청한건지 마틴 파울러의 글은 영 이해가 안됐다.   ~~그 긴글을 읽고 이해할만한 의지가 없었다~~  
그 글을 보고 단숨에 이해할 수 있다면 이걸 읽을 필요는 없다.  

> 글 솜씨도 없고 내 입맛대로 휘갈긴 글이므로 참고용으로만.   ~~더 잘 정리된 글이 분명 어딘가에 있을ㄱ...~~

# IoC, DI, DIP, IoC Container 살펴보기

## IoC (Inversion of Control)

- Hollywood Principle 이라고도 함 (할리우드 원칙, “먼저 연락하지 마세요. 필요하면 연락할게요”)
- 그냥 콜백을 넘기는 게 될 수도 있고, 인터페이스를 구현해서 넘기는 게 될 수도 있고, 여러 가지임
- “넘긴 객체가 호출, 사용되어야 한다/아니다” 혹은 “넘긴 객체를 가지고 있어야 한다/아니다” 등의 생각은 고려되지 않음
- Inline 형태로 존재해야할 코드를 사용하는 놈에게 달라고 떠넘기고 수행하는 놈은 준걸 그대로 호출하도록 바꾸는 개념
- 없던 의존성을 만들어내는 것도 이 개념에 부합함

{{< gist wotjd 85ebad0375531b220252667a305c3d21 >}}

- 자바에서 아래와 같이 쓰는 것도 포함 (Swift에선 안됨)

{{< gist wotjd 188fb930ed9442e45410fab8c8816544 >}}

## DI (Dependency Injection)

- 개념상 IoC 에 포함됨 : **의존성이 이미 있는 상태라는 전제 조건**,   
  의존성이 이미 있는 경우에 클래스 내에서 의존하고 있는 어떤 객체를   
  직접 생성하지 않고 받아서 처리하는 거
- 전혀 어려운 게 아니라 약간 있어 보이기 위한 전문용어 느낌
- 내부 변수를 직접 초기화하지 않고 외부에서 받는 방법
- 인수로 넘기는 방식은 initializer 호출 시 인자, 특정 method 호출 시 인자,  
 프로퍼티 지정의 세 가지 방법이 있어 보임
- 다른 건 다 몇몇 문제가 있고 initializer DI 가 대세인듯함
- 근데 의존성이 많아지면 코드가 길~~어짐 (불-편)   
--> 요거 해결하려고 IoC Container (DI Container) 가 등장

{{< gist wotjd 4e16c33cad6255524c4ee79afbd35c91 >}}

> 추상화 부분은 고려되지 않음 : 추상화 클래스로써 포함하든, 구현 클래스로써 포함하든 상관없음. 그냥 의존성을 분리하는 개념

### Before DI (클래스 내부에서 의존성 객체 생성)

{{< gist wotjd 88998d42348a7dd450041078884df08b >}}

### After DI (initializer DI, 생성자로 의존성 객체 전달 받음)

{{< gist wotjd 81b64993180785e0ac09476693c8ec2d >}}

## DIP (Dependency Inversion Priciple)

- 기본적으로 DI를 베이스로 하는 개념   
(**DI + 추상화** 정도로만 생각하는게 정신건강에 이로울 것 같음, 그 이상은 Too Much)
- DI랑 약간 접근이 다름. DI가 분리의 느낌이라면 DIP는 재사용성 증진을 위한 추상화 활용에 가까움
- 인터페이스(추상화)에선 의존성 정보를 가지고 있으면 안 됨
- 어떤 객체를 전달받으면 그 객체를 활용하는 메소드/코드를 짤 텐데,   
그 객체가 구현 클래스로 박혀있으면 비슷한 동작하는 다른 클래스는 그 메소드/코드를 활용을 못 함.   
그럴 때 넘겨주는 그 객체를 추상화해서 비슷한 동작하는 애들은 다 하나의 인터페이스를 바라보도록 하고,  
 그걸 활용하는 클래스에서는 인터페이스의 메소드만 가지고 구현하는 방법인 것 같음
- 위 설명은 IoC 에도 일부 적용됨, 다른 점은 DIP는 추상화된 객체가 구현 부의 멤버로 선언이 되어 있어야 함 (당연히 의존성이 있으니까)
- **추상화의 효과로 테스트 용이성, 확장성, 다형성 등이 따라온다.**

### Before DIP

{{< gist wotjd e9c34e3e5edf8ce16ef20d20557e9516 >}}

- 피자 종류를 늘려달라는 요구사항이 들어온다면..   
새로운 Pizza 클래스 만들고.. PizzaStore 코드 수정하고..  
OO 씨 다 했어? 그럼 다른 피자도 추가해줘~ ~~ㅁㄴㅇㄹㄴㅇ러먀ㅐㅓ랴~~
- PizzaStore 에서 bakePizzas 할 때 각 Pizza 클래스의 bake 메서드를 쓰는데,  
 새로운 Pizza 클래스에서 bake 메서드가 없다면..?   
 혹은 다른 이름의 메서드로 구현이 되어있다면..? ~~ㄹ냐ㅐ어래냐ㅓ래ㅑㄴ먀ㅐ어~~

### After DIP
{{< gist wotjd cc41cde976abbd03d0ad1aa4db031413 >}}

- 피자 클래스에서 공통적으로 구현이 되어야 할 (PizzaStore 에서 사용해야 하는) bake 메서드를 Protocol 에 선언 (인터페이스 화)
- 모든 Pizza 클래스는 bake 메서드를 구현해야 함 (내부에서 어떻게 구현하고 동작할지는 지들 맘대로)
- PizzaStore 에선 Protocol 에 선언된 함수만 사용
- 뭔가 거창하게 얘기한 것 같지만 그냥 DI + 추상화 한 거

## IoC Controller (Inversion of Control Controller)

- DI에서 이니셜라이저 인자로 뺀 의존성 들을 관리하는 느낌
- DI랑 다른 점은 DI는 구 현부, IoC Controller는 사용부 정도로 생각됨
- 유형은 여러 가지임. 아직 완전히 공부하진 않아서 짧게 찾아봤을 때,   
대표적으로는 Service Locator Pattern이 있는 것 같다. (deprecated 됨)
- 사실 필요성도 잘 못 느끼겠다.. ~~(모르니까 쓰지말자)~~   
Init DI가 된 의존성 많은 클래스를 선언할 때 코드 양을 줄여준다 정도..?   
(근데 Service Locator로 빼는 코드는 생각 안 함..?)

### 예시

{{< gist wotjd eaffd1d9fc3c4f17597c26dd7446f0f8 >}}

# 도움이 될만한 이미지
- IoC, DI의 관계도
![](/res/img/ioc/ioc_diagram.jpg)

- DI 와 IoC 영역 비교 (잘못된 그림)
![](/res/img/ioc/DI-vs-IoC.png)
키워드는 잘 제시됐지만, IoC 가 DI 를 품고 있어야 맞는 그림인 것 같다. (Pure DI 또한 IoC 에 포함이다)  
DI 내에서 Pure DI 와 IoC Container 를 나눈 것은 좋은 생각인 것 같다.

# 참고한 사이트들

## 위키백과는 훌륭한 백과사전입니다.

한글이 아니라 영어일 때. ~~한글문서가 그냥 커피라면, 영어문서는 TOP야..~~

**영어 버전과 번갈아가며 읽어보도록 한다.** ~~근데 나도 다 안읽어봄ㅎ~~

1. [의존성 주입](https://ko.wikipedia.org/wiki/의존성_주입) : Dependency Injection (DI)
2. [의존관계 역전 원칙](https://ko.wikipedia.org/wiki/의존관계_역전_원칙) : Dependancy Inversion Principle (DIP)
3. [제어 반전](https://ko.wikipedia.org/wiki/제어_반전) : Inversion of Control (IoC)

## 꽤 잘 설명된 글들

1. [Dependency Injection 이란?](https://medium.com/@jang.wangsu/di-dependency-injection-이란-1b12fdefec4f) : IoC Container 를 제외하면 최신이고, 예시 코드들이 Swift 였기 때문에 이해하기 좋았음
2. https://ahea.wordpress.com/2018/09/09/1754/ : DIP에 대한 설명은 없지만 IoC의 개념과 DI의 종류가 적절한 예시 코드로 잘 설명되어있음
3. https://edykim.com/ko/post/the-service-locator-is-an-antipattern/ : 서비스 로케이터가 왜 안티-패턴인지에 대한 설명과 코드를 통한 이해 과정, 정리가 잘 되어 있음

## 읽다보면 더 혼란스러워 질 수 있는 글들

근데 다 알고보면 이해가 가기도 한다.

1. https://okky.kr/article/362415 : 의존성 주입이 뭔가요?
2. https://stackoverflow.com/questions/871405/why-do-i-need-an-ioc-container-as-opposed-to-straightforward-di-code : 왜 IoC Container를 써야하니? (의견대립의 현장)
3. https://javacan.tistory.com/entry/120 : 마틴 파울러 번역 글
4. [DI는 IoC를 사용하지 않아도 된다](https://jwchung.github.io/DI는-IoC를-사용하지-않아도-된다) : IoC Container를 그냥 IoC라고 불러 혼동이 있을 수 있지만 그 외는 좋다.
5. [IoC-DI란](https://pks424.tistory.com/entry/IoC-DI란) : 비유는 적절한 부분이 많고 읽어볼만 하지만 DIP 가 빠져있고, 이 글만 읽고 나머지 세가지 요소도 분리해서 이해하기는 쉽지 않았음