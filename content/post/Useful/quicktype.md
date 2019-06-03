---
title: "[Useful] Quicktype.io : Paste JSON as Code"
date: 2019-06-03T11:40:16+09:00
categories:
  - Useful
  - Site
tags:
    - JSON
    - Tools
    - Utils
    - Quicktype
    - Tips
---
https://quicktype.io

JSON 데이터를 입력하면 자동으로 코드를 생성해주는 사이트.

최근에는 네트워크 통신할 때 거의 JSON 을 사용한다.  
JSON 데이터를 코드에서 다루기 위해서는 기본적으로 오고가는 데이터를 맵핑할 모델이 필요하다.  
이 모델을 직접 만들때는 필드 이름의 철자가 한자라도 다르면 오류가 발생하는데,  
파싱해야 할 필드가 많아질수록 타이핑해야 할 코드 수도 증가하여 손이 많이간다.

이럴 때 quicktype 을 사용하면 실수가 많이 줄어들 것 같다.

### 사용법

1. [QuickType](https://quicktype.io) 사이트에 접속하여 우측 상단의 Generate Code 버튼을 눌러 이동한다. 
![](/res/img/useful/quicktype_main.png) *메인 화면*
2. 좌측에 JSON 샘플 데이터를 넣으면 우측에 코드가 생성 된다.
![](/res/img/useful/quicktype_working.png) *우측 상단 주석에 사용법이 있다.*

추가로, 지원하는 언어는 다음과 같다.
![](/res/img/useful/quicktype_language.png)

처음엔 사이트만 있는 줄 알았는데,   
node module, mac app, xcode/vscode extension 으로도 쓸 수 있는 것 같다.
[Appstore 링크](https://itunes.apple.com/kr/app/paste-json-as-code-quicktype/id1330801220?mt=12)