---
title: "Hugo 로 github 블로그 시작하기 - 2. Hugo 시작하기"
date: 2019-05-31T16:48:16+09:00
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
<!--toc-->

# 2. Hugo 시작하기

## 왜 Hugo 인가요

**Jekyll 빌드 환경 구성에 필요한 ruby 를 안써도 된다고 카더라.**
제대로 쓴적도 없지만 ruby의 환경구성 난이도가 세계 라는건 알고있지. Jekyll 은 가볍게 포기  
그리고 **빠르대요.**  ~~삽질한 시간을 생각하면 또이ㄸㅗㅇ...~~

Hugo 가 특히 사용하기 편하다라는 느낌은 못받았다.  
환경 구성, 테마 선택하고 입히는 것도 사실 번거로웠지만  
어차피 다 처음 한번만 하고 말 것들이라서  
참으면서 해보기로 하였다.

## 환경 구성 및 블로그 생성

1. Homebrew 설치 (~~ruby 안쓴다더니 시작부터 사쿠라..~~)  
   `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
2. Go 설치  
   `brew install go`
3. Hugo 설치  
   `brew install hugo`
4. Hugo 페이지 생성  
    `hugo new site [sitename]`

위 과정을 거치고 나니 대략 이런 결과가 출력된다.  
   ```
   Congratulations! Your new Hugo site is created in /Users/YourName/Sites/yoursitename.
   
   Just a few more steps and you're ready to go:
   
   1. Download a theme into the same-named folder.
      Choose a theme from https://themes.gohugo.io/, or
      create your own with the "hugo new theme <THEMENAME>" command.
   2. Perhaps you want to add some content. You can add single files
      with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
   3. Start the built-in live server via "hugo server".
   
   Visit https://gohugo.io/ for quickstart guide and full documentation.
   ```
   따라하면 블로그를 만들 수 있다는 것 같다!

### Hugo Site 구조 (Skeleton)

   생성된 파일은 아래와 같다.
   ```
   archetypes content layouts static config.toml themes data
   ```
   content 내에 페이지를 작성하고 hugo 명령어로 빌드하면 `public` 이라는 폴더가 하나 생기는데,  
   github repository 에는 그걸 올리면 된다는 말씀이시겠다.

## Hugo 명령어

블로그를 만들면서 필수적인 두가지 명령어만 알아보자.

- `hugo` : 빌드 명령어, `public` 폴더 내에 결과물이 생성됨 (markdown to html)
- `hugo server` : 생성한 페이지를 빌드하기 전, localhost 에서 확인해보기 위한 웹서버 실행 명령어

## 참고한 사이트

- [초보자도 쉽게! Hugo 블로그 시작하기](https://movingcoding.tistory.com/3)
- [시리즈 #1 - Hugo 시작하기](https://golangkorea.github.io/post/hugo-intro/getting-started/)
- [HUGO(휴고) 정적 웹사이트 생성하기](https://swhacademy.ga/blog/2019/01/24/hugo-guide/)
- [Creating a Hugo site on Github Pages](https://dev.to/dgavlock/creating-a-hugo-site-on-github-pages-3cjo)
- [Made by HUGO and GitHub Pages](http://brannpark.github.io/blog/post/20151201_hugo_with_github_pages/)

-----
다음은 테마를 선택해볼 것이다.