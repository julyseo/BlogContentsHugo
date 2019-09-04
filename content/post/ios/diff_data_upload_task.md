---
title: "[iOS] URLSession 에서 dataTask 와 uploadTask 의 차이점"
date: 2019-09-04T18:15:43+09:00
categories:
- iOS
- Tips
tags:
- iOS
- Tips
- Swift
- Foundation
- URLSession
- Http
- Networking
- Http 통신
#thumbnailImage: //example.com/image.jpg
---
<!--toc-->
Http 통신을 하기위해서 Alamofire 를 많이 쓴다.   
책이든, 구글이든 Http 통신이라면   
*"Alamofire 는 사용하기 간편하게 구현되어 있으니 Alamofire 쓰세요"* 하는 글을 어렵지 않게 찾아볼 수 있다.

<!--more-->

iOS 개발을 입문한지 얼마안된 나 또한 그런 글들을 많이 봤고   
단순히 Http == Alamofire 이라는 수식을 고정관념처럼 생각하게 되었다.

그러다가 간단한 Toy 프로젝트를 만들 일이 있었는데,   
*이렇게 간단한 Http 통신에 Alamofire 를 굳이 써야하나?* 하는 의문이 들기 시작했다.   
~~(설마 애플이 통신 프레임워크도 제공 안하겠어..)~~

그렇게 찾아보니 Alamofire 는 Foundation 프레임워크의 URLSession 을 랩핑하는 라이브러리라는걸 알게되었다.  
순서는 좀 이상하지만 정말 작은 프로젝트에 라이브러리를 추가하는게 귀찮았던 나는   
Alamofire 대신 URLSession 으로 구현하기 시작했다.

```swift
URLSession.shared.dataTask(with: url) { ... }
```

대충 이런 기본적인 형태로만 사용하고 있었는데,   
계속 dataTask 만 사용하다가 uploadTask 라는 메서드가 보여서  
둘의 차이가 뭔지 궁금해지기 시작했다.

# URLSession Task 종류

그래서 [URLSession 의 설명](https://developer.apple.com/documentation/foundation/urlsession) 을 찾아보니..

> ## Types of URL Session Tasks
>
> Within a session, you create tasks that optionally upload data to a server and then retrieve data from the server either as a file on disk or as one or more `NSData` objects in memory. The `URLSession` API provides three types of tasks:
>
> - *Data tasks* send and receive data using `NSData` objects. Data tasks are intended for short, often interactive requests to a server.
> - *Upload tasks* are similar to data tasks, but they also send data (often in the form of a file), and support background uploads while the app isn’t running.
> - *Download tasks* retrieve data in the form of a file, and support background downloads and uploads while the app isn’t running.

간단하게 요약/해석 해보면

- task 를 생성하면, 세션에서 데이터를 서버로 업로드하고, 서버에서 데이터를 받아온다. 데이터는 파일 형태로 디스크에 받아거나, 메모리에 여러 NSData 형태로 받아올 수 있다.
- task 에는 3가지 종류가 있다.
  - Data task: NSData 로 데이터를 주고 받는다. 서버에 짧고 interactive 한 요청을 하기 위한 task 이다.
  - Upload task: data task 와 비슷(NSData 로 주고 받는 점?)하지만 파일을 보내거나, 앱이 안돌 때 background task 를 지원하기 위한 task 이다.
  - Download task: 파일 형태로 데이터를 받고, 앱이 안돌 때 background 에서 다운로드/업로드 하기 위한 task 이다.

# URLSession 의 종류

그 보다 위 문단에 Session 의 종류도 적혀있다.

> ## Types of URL Sessions
>
> The tasks within a given URL share a common session configuration object, which defines connection behavior, like the maximum number of simultaneous connections to make to a single host, whether to allow connections over a cellular network, and so on.
>
> `URLSession` has a singleton [`shared`](https://developer.apple.com/documentation/foundation/urlsession/1409000-shared) session (which has no configuration object) for basic requests. It’s not as customizable as sessions you create, but it serves as a good starting point if you have very limited requirements. You access this session by calling the shared class method. For other kinds of sessions, you instantiate a [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession) with one of three kinds of configurations:
>
> - A *default session* behaves much like the shared session, but allows more configuration, and allows you to obtain data incrementally with a delegate.
> - *Ephemeral sessions* are similar to shared sessions, but don’t write caches, cookies, or credentials to disk.
> - *Background sessions* let you perform uploads and downloads of content in the background while your app isn't running.
>
> See [Creating a Session Configuration Object](https://developer.apple.com/documentation/foundation/urlsessionconfiguration#1660412) in the [`URLSessionConfiguration`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration) class for details on creating each type of configuration.

영문 투성이지만 천천히 해석해보자면,

- URLSession 은 기본적인 요청들을 위해 shared 라는 공용 세션을 공유한다.
- 이 공용 세션은 설정할 수 있는 옵션이 적으니 여러 설정이 필요하면 직접 세션을 만들어야 한다.
- 세션 종류는 세가지가 있다.
  1. default 세션: shared 세션하고 비슷하게 동작하는데, 옵션이 더 많고 delegate 설정도 가능함
  2. Ephemeral 세션: shared 세션하고 비슷한데, 캐시/쿠키/인증서? 가 디스크에 저장되지 않음 => 브라우저의 시크릿 모드와 비슷해 보임
  3. Background 세션: 말그대로 앱이 안돌때도 동작하는 백그라운드 세션이다.

------

기본적인 기능만 사용하다가 궁금해서 찾아봤는데,  
이외에도 생각보다 많은 기능을 제공하고 있어서 앞으로 더 많은 공부가 필요해보인다.