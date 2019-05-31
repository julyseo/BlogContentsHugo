---
title: "Hugo 로 github 블로그 시작하기 - 3. 테마 선정 및 커스터마이징"
date: 2019-05-31T17:00:16+09:00
categories:
  - Blogging
tags:
  - Blogging
  - Blog
  - 블로그
  - Hugo
  - Github
  - Github Pages
# thumbnailImagePosition: left
# thumbnailImage: //d1u9biwaxjngwg.cloudfront.net/highlighted-code-showcase/peak-140.jpg
# draft: true
---
Hosting Blog via Github Pages with Hugo on Mac, 맥에서 Hugo 로 Github Pages 에 블로그 호스팅하기

Hugo 는 포스팅 하는 당시의 최신버전을 사용한다. (v0.55.6/extended darwin/amd64)  
Tranquilpeak 는 [c7d2fde](https://github.com/kakawait/hugo-tranquilpeak-theme/tree/c7d2fdee5d258d95089e0ee73333f37e6e823e0d) 커밋을 포크하여 커스터마이징 하였다. (0.4.4-BETA ~)
<!--toc-->

# 들어가기에 앞서

가장 많은 삽질을 한 구간이다.

특히 오픈소스 테마에서 문서화 되지 않은 부분들에 대한 수정과  
구글링을 해도 해법이 나오지 않는 *특별한*  에러는 날 미치게하였다.

결과적으로는 어느정도 불편하지 않을 정도로만 수정하도록 타협하여 적용했다.  
삽질을 많이 한 만큼 꽤나 긴 문서가 될 것 같다.

# 3. 테마 선정 및 커스터마이징 ~~삽질~~

https://themes.gohugo.io (Hugo Themes) 에서 여러가지 테마를 고를 수 있다.

위 사이트에서 '난 미니멀리즘을 지향해' 라는 생각을 가지고 가장 간단한 테마를 골랐다.  
  
![](/res/img/blogging/hello_friend_ng_showcase.png) [Hello Friend NG](https://github.com/rhazdon/hugo-theme-hello-friend-ng)

> 뭔가 아쉽다.. 카테고리별로 나누고.. 뎁스별로 이동하고 싶은데..

그렇게 시작한 삽질..  
내가 원하는 것은 간단했으나 Hugo 테마 구성은 결코 간단하지 않았다.

대충 템플릿에 Section Variable 들을 이용하면 사이트에서도  
폴더별로 나눠서 분류할 수 있겠다 싶었지만, css 가 발목을 잡기 시작했다.

몇시간 씨름해봤지만 결국 포기하고 다른 (기능이 많은) 테마를 찾아 나서던 도중,  
  
![](/res/img/blogging/tranquilpeak_showcase.png) [Tranquilpeak](https://github.com/kakawait/hugo-tranquilpeak-theme.git) 을 찾았다.

기존의 Hexo 용 테마를 어떤 능력자께서 Hugo 용으로 포팅 중인 테마다. (같은 사람인가..?)

알파, 베타 버전 밖에 없는 만큼 버그가 꽤 있겠지만, 
이미 완성형 디자인인데다가 내가 원하는 기능을 기본지원 했기 때문에 선택했다.

## Transquilpeak 테마 적용

기본적으로 테마 적용은 너무나도 쉽다.  
적용 과정을 순서대로 나열해보자면 다음과 같다.

1. 테마 다운로드 (git clone 도 괜찮고, github 페이지에서 zip 형태로 받아도 된다.)  
`git clone https://github.com/kakawait/hugo-tranquilpeak-theme.git`
2. themes 폴더로 복사  
`cp -r hugo-tranquilpeak-theme [path/to/blog]/themes`
3. config.toml 내에 테마 선언  
`theme = "hugo-tranquilpeak-theme"`
4. 빌드/실행  
`hugo server`

하지만 바로 확인해보니 몇몇 문제가 보인다. 
한글 출력이 제대로 되지 않았고, 폰트도 맘에 들지 않는다.

## 한글 출력

config.toml 내에 아래와 같이 설정한다.

```toml
languageCode = "ko-kr"
defaultContentLanguage = "en-us"
```

defaultContentLanguage 를 ko-kr 이 아닌 en-us 로 설정하는 이유는  
tranquilpeak 테마의 이슈인지는 모르겠지만 날짜 표기가 제대로 되지 않는 문제가 있기 때문이다.  
(위와 같이 설정해도 실제 컨텐츠 한글 표기에는 문제 없음)

## 폰트 변경 (삽질 시작)

다른 설정과 마찬가지로 폰트 또한 config 에 명시하고, 폰트 파일을 옮겨놓는 정도로 쉽게 변경이 가능할줄 알았다. 
그러나 문서에 별다른 내용이 보이지 않아  
git repo 내 issues 탭에서 검색을 해봤더니 아래와 같은 링크가 있었다.

[How to set custom font in this theme?](https://github.com/kakawait/hugo-tranquilpeak-theme/issues/52)  
요약 : config엔 그런 옵션 없으니까 테마 커스터마이징 해야되고, scss 파일 수정하고 테마 빌드해봐 ㅎㅎ

![](/res/img/blogging/user_md_easy_custom.png) *테마 빌드..??? Easily cusomizable 이라며.. 왠지 통수맞은 기분.. (~~그러나 시작에 불과했다~~)*

### 테마 빌드환경 구성

그리하여 볼 생각도 없었던 developer 문서를 보니,

![](/res/img/blogging/developer_md_requirements.png) 

오호 빌드 환경은 Hugo 랑 Grunt CLI 만 미리 설치해놓으면 나머진 npm install 로 설치가 되는구나!  
그럼 requirements 에 node 랑 npm 도 써놔야 될 거 아냐.. 이미 깔려있으니까 참는다..

굳굳 바로 해봐야징

`npm install`
...
![](/res/img/blogging/build_error.png) *아름답다*

~~cpp 빌드는 또 왜..~~  
역시 한번에 되는게 없다. 빌드 로그를 확인해보니

![](/res/img/blogging/build_error_2.png)

이런게 몇개 떠있는 걸 보니 IsNearDeath 가 없는 것 같은데 잘보니 v8..??

![](/res/img/blogging/node_version.png)

내가 쓰는건 v12인데.. 호환성 문제인가.. 싶어서 구글링해보니  
참고할만한 두 자료가 있었다.

1. [Unable to change fonts](https://github.com/kakawait/hugo-tranquilpeak-theme/issues/259)  
요약 : 테마 소스 수정한 뒤에는 빌드 해야되고 노드 8.9.4 에선 빌드 잘 되더라
2. [Mac에서 Node 7 버전으로 다운그레이드](https://johngrib.github.io/wiki/trouble-shooting-node-7-install/#nodejsorg에서-다운로드---성공)  
요약 : node 7 버전은 brew 에 없어서 수동설치 해야하지만 8 버전은 brew 로 설치 가능해 ㅎㅎ

이 외에도 자료가 있었지만 'node 12 도 지원해주세요' 하는 글 뿐이었으므로 호환성 이슈임을 알 수 있었다.  
따라서 잘된다는 노드 8 버전대로 다운그레이드를 해본다.

```zsh
> brew install node@8
some install logs...

node@8 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have node@8 first in your PATH run:
  echo 'export PATH="/usr/local/opt/node@8/bin:$PATH"' >> ~/.zshrc

For compilers to find node@8 you may need to set:
  export LDFLAGS="-L/usr/local/opt/node@8/lib"
  export CPPFLAGS="-I/usr/local/opt/node@8/include"
```
brew 로 설치를 하고 나니 뭔가 뜨는데, 대충 보니 대체버전이니까 링크 안걸어놨다는 뜻.

node 8 버전으로 쓰려면, 위에 명시된대로 rc 파일에 추가를 해놓던가,  
필요할 때 마다 export 해서 쓰는 방법이 있겠다.  

컴파일 용이라면 LDFLAGS 와 CPPFLAGS 도 같이 export 를 해주어야 하는데,  
이것도 마찬가지로 rc 파일에 명시하던가, 필요할 떄 마다 export 해주면 된다.  

보통은 node 8 보다 최신버전을 쓰는 상황이 많아 후자로 진행한다.
```zsh
> export PATH="/usr/local/opt/node@8/bin:$PATH"       # node 명령어 사용을 위한 bin path 선언
> export LDFLAGS="-L/usr/local/opt/node@8/lib"        # cpp 빌드를 위한 라이브러리 path 선언
> export CPPFLAGS="-I/usr/local/opt/node@8/include"   # cpp 빌드를 위한 헤더파일 path 선언

> node -v
v8.16.0
```
이제 다시 `npm install` 을 하면 환경 구성이 되고,
폰트 수정 후 `npm run prod` 를 실행하면 테마가 빌드된다.

**결론 : node 최신버전으로는 안되니 8.대 버전을 이용하여 빌드하자**

> developer.md 의 Requirements 에 node 버전 한 줄만 적혀있었어도 삽질하는 시간이 줄었을텐데..

### Tranquilpeak 테마의 폰트 수정하기

[Tranquilpeak 테마에 웹 폰트 적용하기](http://blog.lattecom.xyz/2016/05/08/tranquilpeak-theme-web-font/) 를 참고하였다.

내가 적용한 방식을 간략하게 나열하면 다음과 같다.

1. `tranquilpeak/src/scss/utils/_fonts.scss` 에 사용할 폰트 import  
![](/res/img/blogging/import_font.png)

2. `tranquilpeak/src/scss/utils/_variables.scss` 에 폰트 선언 및 요소별 사용 폰트 지정  
![](/res/img/blogging/change_font.png)

3. 빌드 및 사용  
`npm run prod`

## 참고한 사이트

- [How to set custom font in this theme?](https://github.com/kakawait/hugo-tranquilpeak-theme/issues/52) 
- [Tranquilpeak User.md](https://github.com/kakawait/hugo-tranquilpeak-theme/blob/master/docs/user.md)
- [Tranquilpeak Developer.md](https://github.com/kakawait/hugo-tranquilpeak-theme/blob/master/docs/developer.md)
- [Tranquilpeak 테마에 웹 폰트 적용하기](http://blog.lattecom.xyz/2016/05/08/tranquilpeak-theme-web-font/)
- [Unable to change fonts](https://github.com/kakawait/hugo-tranquilpeak-theme/issues/259)  
- [(문제해결) Mac에서 Node 7 버전으로 다운그레이드](https://johngrib.github.io/wiki/trouble-shooting-node-7-install)

-----
다음은 테마, 컨텐츠, 페이지 관리를 위한 Github Repository 를 생성할 것이다.