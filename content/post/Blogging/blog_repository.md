---
title: "Hugo 로 github 블로그 시작하기 - 4. Repository 관리 및 Github Pages 호스팅"
date: 2019-05-31T18:38:16+09:00
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
Hosting Blog via Github Pages with Hugo on Mac, 맥에서 Hugo로 Github Pages에 블로그 호스팅하기

Git에 대한 충분한 지식이 있다는 전제하에 적은 문서이다.
<!--toc-->

# Repository 관리 및 Github Pages 호스팅

## Repository 관리 (SubRepo 지정)

모든 리소스를 하나의 Repository로도 관리가 가능하겠지만  
보통은 컨텐츠 Repository, 호스팅 될 Repository로 나눈다.

나는 후에 Hugo Build를 자동화하기에도 용이할 것이라고 판단하여 두 개의 저장소를 생성했다.

컨텐츠 Repository에는 Hugo 관련 파일 + 컨텐츠 파일들이 들어있고,  
호스팅 될 Repository에는 hugo 빌드 결과물만 존재한다.

지금은 테마도 커스터마이징 했기 때문에   
원본 Repository 를 fork 하여 별도의 Repository에서 관리를 해야 한다.  

결과적으로는 아래와 같은 형태로 관리하고 있다.  
![](/res/img/blogging/github_repos.png) 


- 컨텐츠 Repo, 호스팅 될 Repo를 따로 둔다.
- 테마 Repo 는 원본을 Fork 하여 관리한다.
- 컨텐츠 Repo 에는 테마 Repo를 SubRepo 로 지정  
`git submodule add [git Repo].git path`
- 컨텐츠 Repo 에 travis ci (빌드 자동화)를 세팅, target은 호스팅 될 Repo로 지정

위와 같이 3개의 Repository로 구성하고, 컨텐츠 Repository에 Travis를 설정하여  
컨텐츠 Repository가 커밋되는 경우 Travis를 통해 Hugo 빌드를 하고,  
빌드된 결과물을 호스팅 될 Repository에 자동으로 Deploy 한다.

이렇게 하면 컨텐츠 Repo 는 커밋 한 번으로 빌드/블로그 반영이 자동으로 되는 것이다.

이런 경우는 레포를 나눈 게 커밋 한 번으로 블로그에 반영하기 위해서인데,  
실제로는 어떻게 구성하든 상관없다.  
구성 및 추가될 것들을 고려하여 효율적으로 나누면 그만인 것이다.

> Travis 자동화에 관한 것은 이후 포스팅에서 살펴볼 것이다.

## Github Pages 호스팅
### Github Pages란?
https://pages.github.com (공식 사이트 링크)

Github Pages는 Github 에서 제공하는 무료 호스팅 서비스다.  
Page에는 다음과 같이 두 종류가 있다.  
<br>

1. User, Organization Page
   - 제약 : 호스팅 될 Repository 이름을 *[유저, 조직 이름].github.io* 로 설정해야 함 (master 브랜치)
   - 주소 : **[유저, 조직 이름].github.io**
2. Project Page
   - 제약 : *gh-pages* 브랜치를 생성하여야 함
   - 주소 : **[유저, 조직 이름].github.io/[project name]**

위 제약 사항을 지키지 않으면 Github Pages를 생성할 수 없으며,  
**Repository 관리** 에서 언급된 호스팅 될 Repo를 따로 생성하는 방식은 1번에 해당한다.

위와 같이 Repository를 생성하거나, 브랜치를 생성한 이후에는  
index.html만 포함하여 커밋해도 페이지에 접속할 수 있다.  

### 정적 사이트 생성기 (SSG ~~신세계 광고 아님~~, Static Site Generator)

github pages에서는 Jekyll을 기본적으로 지원하기 때문에   
형식에 맞춰 Markdown을 작성해서 올리면  
별도의 작업 없이도 페이지를 볼 수 있고,   
Repository Setting - Choose Theme을 통해 테마를 선택하면   
Jekyll을 기반으로 한 페이지가 쉽게 생성된다.

그래서 별도 작업 없이 Github Pages를 사용할 땐,  
원래부터 Markdown 을 브라우저에서 인식하고 페이지를 보여주는 것처럼 착각할 수 있는데,  
사실은 github pages에 Markdown을 올리면 내부적으로 Jekyll을 통해 빌드되어   
빌드가 된 파일을 가지고 호스팅을 하는 것이다.

이렇게 Markdown을 기반으로 페이지를 생성하기 위해  
Hugo/Jekyll과 같은 **정적 사이트 생성기(Static Site Generator)**를 사용하는 것이다.  
(정적 사이트 생성기의 종류는 https://www.staticgen.com 에서 확인할 수 있다)

정적 사이트 생성기는 사용하는 문법이나 사용 가능한 요소가 조금씩 다르기는 하지만  
기본적으로 **template(테마), config, markdown**을 바탕으로  
브라우저에서 읽어 들일 수 있는 전형적인 형태인 **css, html, js**기반의 페이지를 생성한다.

여기에서는 Hugo를 사용하여 페이지를 만들 것이므로  
Hugo로 Github pages 호스팅하는 과정을 간단하게 살펴보면 다음과 같다.  
<br>

1. hugo 빌드하기 `hugo`  
![](/res/img/blogging/hugo_build.png) *Warning이 있지만, Count 가 출력되면 정상 빌드된 것이다.*
2. 빌드 된 결과물을 Repository에 커밋  
![](/res/img/blogging/public.png) *빌드하면 public 내에 페이지 파일들이 생성된다.*
3. 커밋을 한 뒤 수 분 뒤에 페이지 확인  
![](/res/img/blogging/wotjd_github_blog.png) *문제없이 잘 나온다!*

## 참고한 사이트

- [User, Organization, and Project Pages](https://help.github.com/en/articles/user-organization-and-project-pages)
- [StaticGen](https://www.staticgen.com)
- [github로 무료 웹호스팅, 미디어 호스팅 이용하기(for 디자이너)](http://koreawebdesign.com/github-web-hosting-build-guide/)

-----
지금까지의 내용을 간략하게 정리하면 아래와 같다.  
<br>

1. Hugo 환경 구성 및 테마 커스터마이징
2. 테마 변경 사항을 테마 관리 Repository에 커밋
3. content 내에 Markdown 기반 문서 작성
4. `hugo server`로 확인
5. 변경사항을 컨텐츠 Repository에 커밋
6. `hugo` 명령어로 빌드
7. 빌드 결과물을(public) 호스팅 될 Repository에 업로드 (커밋)

관리하는 Repository가 많아져 수동으로 커밋을 몇번씩 해야하고  
Hugo 빌드도 직접 해야 하는데,  
이제 이 과정들을 Travis CI 연동을 통해 자동화하여 줄여나갈 것이다.