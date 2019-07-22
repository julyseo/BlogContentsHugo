---
title: "[iOS] 문자열 치환 (특정 문자열 제거, Replace String in Swift)"
date: 2019-07-22T12:09:16+09:00
categories:
- iOS
- Tips
tags:
- iOS
- Tips
- Swift
- String
- Replace String
- 문자열 치환
- 특정 문자열 체거
#thumbnailImage: //example.com/image.jpg
---

앱을 개발하다보면 문자열에서 특정 문자열을 제거/변경하여 사용해야 하는 경우가 더러 있다.  

여러가지 방법이 있겠지만 나는 `replacingOccurrences` 메소드를 사용하는 방법을 선호한다.  

**UUID 문자열에서 하이픈 ('-') 제거하는 예제**
{{< gist wotjd 09bafdb23cc746d6e2ccd5c645ae95d1 >}}

---

## func replacingOccurrences(of target: String, with replacement: String, options: NSString.CompareOptions = [], range searchRange: NSRange) -> String

***지정된 범위에서 target 문자열을 replacement 문자열로 바꿔 새로운 문자열을 반환함***   
*(Returns a new string in which all occurrences of a target string in a specified range of the receiver are replaced by another given string)*

**options 와 range 는 생략 가능함**