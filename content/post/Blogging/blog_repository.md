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
draft: true
---
Hosting Blog via Github Pages with Hugo on Mac, 맥에서 Hugo 로 Github Pages 에 블로그 호스팅하기
<!--toc-->

# 4. Repository 관리 및 Github Pages 호스팅

## Repository 관리 (Subrepo 지정)

모든 리소스를 하나의 레포지토리로도 관리가 가능하겠지만  
보통은 컨텐츠 관리, 호스팅 될 Repository 로 나눈다.

나는 후에 Hugo Build 를 자동화하기에도 용이할 것이라고 판단하여 두 개의 저장소를 생성했다.

컨텐츠 관리 하는 쪽에는 Hugo 관련 파일 + 컨텐츠 파일들이 들어있고,  
호스팅 될 Repository 는 hugo 빌드 결과물만 존재한다.

지금은 테마도 커스터마이징 했기 때문에   
원본 Repository 를 fork 하여 별도의 레포지토리에서 관리를 해야한다.  

결과적으로는 아래와 같은 형태로 관리하고 있다.

- 컨텐츠 관리 repo, 호스팅 될 repo 를 따로 둔다.
- theme repo 를 fork 하여 관리
- 컨텐츠 관리 repo 에는 theme repo 를 subrepo 로 지정  
`git submodule add [git repo].git path`
- 컨텐츠 관리 repo 에 travis ci (빌드 자동화)를 셋팅, target 은 호스팅 될 repo 로 지정

위와 같이 3개의 Repository 와 Travis 를 설정하여  
컨텐츠 관리 Repository 가 커밋되는 경우에 travis 에서 Hugo 빌드를 하고,  
빌드된 결과물은 호스팅 될 Repository 로 Deploy 된다.

이렇게 구성하여 아래와 같은 상황에 자동화를 통해 github 블로그에 자동 반영 된다.

- 컨텐츠 관리 repo 에서 글 작성 후 커밋
- 테마가 수정되는 경우 컨텐츠 관리 repo 에서 submodule 업데이트 및 커밋


테마가 수정되는 경우 수동으로 submodule 을 업데이트 하고 커밋해야 하지만,  
일단은 컨텐츠 관리 repo 에 커밋 한번으로 자동 빌드되어 블로그에 반영이 되니 만족스럽다.

시간이 나는대로 테마 repo 에도 수정 시 자동으로 submodule 을 업데이트 하고 커밋되도록 travis 를 설정해야겠다.

travis 구성으로 얘기가 끝났는데,  
레포를 쪼개서 관리하는 것은 "커밋 한번으로 블로그에 반영" 를 목적으로  
직관적인 빌드 자동화의 초석을 깔아 놓는다는 생각으로 접근했기 떄문인 것 같다.

자세한 travis 구성은 이후 포스팅 에서 살펴볼 것이다.

> 어떻게 구성하든 상관없지만, 계속 추가될 것, 수정될 것들을 고려하여 적절하게 쪼개놓는 것이 관건인 것 같다.

## Github Pages 호스팅

Github Page 는 Github 에서 제공하는 무료 호스팅 서비스다.  
Page 에는 다음과 같이 두 종류가 있다.

1. User, Organization Page
   - 제약 : repository 이름을 [유저, 조직 이름].github.io 로 설정해야 함 (master 브랜치)
   - 주소 : [유저, 조직 이름].github.io
2. Project Page
   - 제약 : gh-pages 브랜치를 생성하여야 함
   - 주소 : [유저, 조직 이름].github.io/[project name]

위 제약 사항을 지키지 않으면 Github Pages 를 생성할 수 없으며,  
**Repository 관리** 에서 언급된 호스팅 될 repo를 따로 생성하는 방식은 1번에 해당한다.

Repository 생성 이후에는 index.html 만 포함하여 커밋해도 페이지에 접속할 수 있다.  

Jekyll은 github pages 에서 기본적으로 지원하기 때문에 Jekyll 형식에 맞춰 Markdown 을 작성해서 올리면  
별도의 작업 없이도 볼 수 있고, Repository Setting - Choose Theme 에서  
테마를 선택하면 Jekyll을 기반으로 한 페이지가 생성된다.

일반적인 Markdown 은 당연히 지원이 안되는데,  
작성하기 편리한 Markdown 을 기반으로 페이지를 생성하기 위해  
Hugo/Jekyll 같은 정적 사이트 생성기(Static Site Generator)를 사용하는 것이다.

정적 사이트 생성기는 사용하는 문법이나 사용 가능한 요소가 조금씩 다르기는 하지만  
기본적으로 template (theme), config, markdown 을 바탕으로  
브라우저에서 읽어들일 수 있는 전형적인 형태인 css, html, js기반의 페이지를 생성한다.

여기서는 Hugo 를 사용하기로 했으므로 Hugo 를 사용하여 페이지를 생성하고  
Github Pages에 반영하는 과정을 살펴볼 것이다.


repository 에 커밋을 하면 수 분뒤에 페이지가 반영이 된다.

## 참고한 사이트

-----
지금까지의 내용을 간략하게 정리하면 아래와 같다.

1. Hugo 환경 구성 및 테마 커스터마이징
2. 테마 변경 사항을 테마 관리 Repository 에 커밋
3. content 내에 Markdown 기반 문서 작성
4. hugo server 로 확인
5. 변경사항을 컨텐츠 관리 repository 에 커밋
6. hugo 명령어로 빌드
7. 빌드 결과물을(public) 호스팅 될 repository 에 업로드 (커밋)

관리하는 Repository 가 많아져서 그런지 많이 복잡하다.  
이제 이 과정들을 Travis CI 연동을 통해 자동화하여 줄여나갈 것이다.