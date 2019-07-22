# [Swift] Functor 와 Monad, map 과 flatMap

## 간단하게,

Functor 와 Monad 는 다른 타입을 감싸는 개념이다.   
다른 타입을 감싸는건 컨텍스트(Context)라고 하며 감싸지는 것을 컨텐트(Content)라고 한다.  
컨텍스트는 컨텐트를 담는 컨테이너 개념인 것이다.

실제 코딩을 할 때는 map 을 사용할 수 있다면 Functor 이고,   
flatMap 을 사용할 수 있으면 Monad 이다.

## 라는 설명은 이해가 가지 않았다.

map, flatMap 이 뭐길래 Functor, Monad 를 나누며,  
어떤 곳에서는 Context 가 값이 있을 수도 있고, 없을 수도 있다면서  
Monad 를 설명할 때에만 컨텐트가 있을지 없을지 모르는 상태까지 포함한다고 써놨다.  
심지어 Monad = Functor + Context 라고..

**Functor 도 Context 인데!**

### 있을지 없을지 모르는 상태? 그거 완전 Optional 아니냐?

맞다. Optional 은 Swift 에서 Monad를 구현한 대표적인 컨텍스트다.   
있을지 없을지 모르는 상태를 표현할 수 있고 (Optional.none, Optional.some)  
map 과 flatMap 함수까지 제공한다.

**그렇다. Optional 은 Monad 이다.**

### 없는 값, nil

없는 값, Objective-C 와 Swift 에서는 nil 이라고 표현한다.  
Objective-C 시절에는 다른 언어의 null 처럼 "없는 값"을 Optional 같은 장치없이 표현할 수 있었다.   
Swift 에서는 안전 상의 이유로 (Runtime 에서의 Null Pointer Exception, NPE 를 방지하기 위해)   
Optional 을 도입하면서 <타입>? 없이는 nil 을 사용할 수 없게 끔 컴파일러 룰로써 추가했다.

다시말해, "없는 값"이란 개념이 애초에 없었는데 Optional 로 추가해준 것이 아닌,  
원래부터 있던 nil 을 Optional 을 쓰지않으면 사용하지 못하게끔 (빌드가 실패하도록) 한 것이다.   
나는 이것을 ***nil 표현의 제한*** 이라고 이해했다.

### 그래서 헷갈린다.

Monad 를 설명하는 많은 글이 대표적으로 Optional 을 예로 든다.  
Monad 인 Optional 을 사용하지 않으면 nil 을 다룰 수가 없으니까.  
Swift 에선 Functor 를 구현하려고 해도 Monad 인 Optional 을 써야되고  
Monad 를 구현하려고 해도 Monad 인 Optional 을 써야한다.

~~Monad 인 Optional 로 Monad 를 구현한다~~

### Optional 은 이제 그만

Optional 이 Monad 인 이유는 이제 그만해도 알겠다.  
Monad 를 떠올릴 때마다 Optional 이 튀어나와서 생각을 방해하니  
이제부터 Optional 은 없는 값(nil) 을 표현하기 위한 도구, 그 이상도 이하도 아니다.

우리는 Functor 와 Monad 의 조건을 좀 더 살펴볼 것이다.  

## Functor 와 Monad

[Functor](https://ko.wikipedia.org/wiki/함자_(수학)) 와 [Monad](https://en.wikipedia.org/wiki/Monad_(category_theory)) 는 범주론(Category theory)에서 나온 개념이다.   
따라서 특정 조건의 함수식을 만족해야하며 Monad 는 아래와 같은 식을 만족해야한다.

![](/res/img/ios/monad_theory.png)   
..ㅎㅎ 잘 모르겠다. 근데 문서가 하나 더 있다..?

[Monad (functional programming)](https://en.wikipedia.org/wiki/Monad_(functional_programming))   
![](/res/img/ios/monad_wiki_functional.png)   
..ㅎㅎㅎㅎㅎㅎ 너가 말로만 듣던 하스켈이구나..?  

더 모르겠다. 모나드의 이해가 어렵단건 이런거였구나..^^

그런데, 간단한 정의를 이미 봤다.

***map 을 사용할 수 있으면 Functor 이다.***   
***flatMap 을 사용할 수 있으면 Monad 이다.*** 

Functor 와 map, 그리고 flatMap 과 Monad 는 대체 어떤 관계에 있는 것일까?

### Functor, 그리고 map

Functor 의 어려운 수식을 해석해보면 다음과 같다.   
   
보통 map 을 쓸 때는 아래와 같이 for 문을 대신하여 사용한다.  
나 역시도 이렇게 사용하고 있었다.  
그도 그럴 것이 map 의 동작을 이해함에 있어  
"for 문으로 돌릴 코드를 map 으로 단 몇줄에 끝낼 수 있어!"   
라고 설명하는 글이 정말 많기 때문이다.

이건 map 을 iterator 로써만 생각하고 사용하는 것이다.

map 의 return 결과를 자세히 살펴보면 다음과 같다.

### Monad, 그리고 flatMap

## 사실은 몰라도

사실은 몰라도 개발자의 감으로 다른 사람들이 쓰는 것처럼  
map 과 flatMap 정도는 쓸 수 있다. (그렇게 쓰고 있었다)

그러나 이런 개념을 알고 map 과 flatMap 을 마주한다면  
사뭇 다르게 보일 것이고, map 과 flatMap 을 사용하여   
이전과는 다른 효율적인 코딩이 가능할 것이다.

## 도움이 된 링크



