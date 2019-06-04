---
title: "[Blogging] Hugo 로 github 블로그 시작하기 - 5. Travis CI 연동"
date: 2019-06-04T16:34:47+09:00
categories:
  - Blogging
  - 블로그 시작하기
tags:
  - Blogging
  - Blog
  - Hugo
  - Github
  - Github Pages
  - Travis
  - 블로그
  - 휴고
  - 빌드 자동화
#thumbnailImage: //example.com/image.jpg
---

Hosting Blog via Github Pages with Hugo on Mac, 맥에서 Hugo로 Github Pages에 블로그 호스팅하기

Git에 대한 충분한 지식이 있다는 전제하에 적은 문서이다.
<!--toc-->

# Travis CI 연동

Travis CI는 지속적 통합(Continuous Integration, CI)을 위한 빌드, 테스트 등을 자동화하는 툴이다.

여기서는 간단하게 컨텐츠 관리 repo 를 커밋했을 때 빌드 후 호스팅 될 repo 에 deploy 하도록 구성한다.

## Travis CI 설정

[Travis](https://travis-ci.org) 에 로그인하여 Account Setting 의 Legacy Services Integration 에서   
컨텐츠 관리 repo 의 스위치를 on 시켜준다.

![](/res/img/blogging/travis_setting.png) *해당하는 Repository 를 체크한다.*

Travis CI 는 repository 내 root 디렉토리에서 .travis.yml을 읽어들여 수행한다.  
Integration을 설정했다면 .travis.yml 을 작성하여 빌드/배포를 수행하도록 할 수 있다.

여기에서는 Hugo 로 빌드하고 호스팅 될 repository 에 배포하기 위해 .travis.yml 를 작성한다.  
자동화할 항목은 

1. Hugo build
2. deploy to target repo  

정도가 있는데, 2번 과정에서 Github Token 이 필요한 것 같다.

## Github Token 발급 및 Travis 환경 변수 등록

[GitHub](https://github.com) 에 로그인하고 Settings - Developer settings - Personal access tokens 에서   
Generate new token 을 클릭하여 토큰을 발급 받는다.

![](/res/img/blogging/gen_new_token.png) Generate new token 클릭

![](/res/img/blogging/new_token1.png) repo 를 선택

![](/res/img/blogging/new_token2.png)스크롤을 내려 Generate token 클릭

![](/res/img/blogging/generated_token.png) token 발급 완료

발급 받은 토큰을 복사하여  
Travis 의 Repository Setting 내 환경 변수 (Environment Variables) 로 추가해준다.

- Name : GITHUB_TOKEN  
- Value : [발급받은 토큰 값]  
- Display value in build log 는 미체크 (보안 이슈)  

기재 후 Add 버튼 클릭

![](/res/img/blogging/add_token_var.png)

여기에서 Name 은 임의로 정해도 된다.  
이렇게 환경 변수를 설정하면, .travis.yml 에서 사용할 수 있다.  
(당연하지만 사용하는 변수명은 설정한 Name 과 동일해야한다)

## .travis.yml 작성/테스트 및 build/deploy 시간 단축

### go get 명령어를 사용하는 방법
구글링 해 본 결과 go get 을 사용하라는 글이 가장 많았다.

{{< gist wotjd 1ef3b74ca72a263a6038528aa8c2e775 >}}

github_token 에 환경 변수에 추가한 `GITHUB_TOKEN`을 사용한다.  
이대로 커밋하면 잘 되어야 하는데.. 아래와 같이 Command Error 가 발생한다.

```
The command "go get github.com/spf13/hugo" failed and exited with 1 during .
```

현재 hugo 의 레포지토리 주소가 gohugoio/hugo 로 바뀌기는 했지만, spf13/hugo 로도 clone 은 잘 된다.

go 명령에서 실패했으니 호환성 이슈인가 싶어 hugo 의 Readme 를 확인해보니  
적어도 1.11 버전은 써야하는 것으로 보인다. ([Prerequisite Tools](https://github.com/gohugoio/hugo#build-and-install-the-binaries-from-source-advanced-install))  
![](/res/img/blogging/hugo_build_requirements.png)


그리고, hugo 의 travis 설정 파일을 보니 

```yml
go:
  - "1.11.10"
  - "1.12.5"
  - tip
```

로 되어있다.

go master 로 사용하면 자동으로 latest 버전을 사용한다 하는데, 아닌가 싶어 버전을 명시하도록 수정하였다.

```yml
go:
   - "1.11.10"
```

다시 커밋해보니 빌드는 성공하여 target repo 에도 커밋이 생기고 블로그에 반영까지 됐으나,  
**deploy 까지에만 약 3분 30초 가량이 소요된다.**

그 중 **go get 명령어에서만 2분 30초 가량 소요**되는데,  
너무 오래걸리는 것 같아 다른 방법을 찾기 시작했다.

그러다가 Command Error 가 났을 떄 찾은 자료를 얼핏 보니 go get 대신   
이미 릴리즈된 바이너리를 받아 쓰라는 내용이 있었다. ([stackoverflow](https://stackoverflow.com/questions/47620143/travis-error-the-command-go-get-u-v-github-com-spf13-hugo-failed-and-exited))

그래서 아래와 install 에 같이 가장 최근에 릴리즈된 0.55.6 버전을 직접 받아 사용하도록 수정하였다.

{{< gist wotjd e90f807e988895f5b93fecb92f9efc36 >}}

1분 내로 deploy 까지 완료 된다!!  
버전을 명시해놓는게 썩 좋은 방법 같지는 않지만..

travis 가 가상머신(아마 docket) 기반으로 돌아가는데,  
system_info 를 확인해보니 ubuntu 14.04 였다.

이를 이용해서 install 부분을 더 간단하게 `sudo apt-get install hugo`  
같이 OS 패키지 매니저에서 관리되는 최신버전을 받아오도록 할 수도 있을 것 같지만,  
잠깐 시도해보니 잘 안되는 것 같다.

일단은 위 방법으로도 만족할만한 결과가 나왔으니 당분간은 이렇게 쓰고  
조금 더 시간을 들여 travis 설정도 천천히 살펴봐야겠다.

## 참고한 사이트

- https://ironpark.github.io/2017/12/17/hugo-site-deploy-use-travis-ci/
- https://kdy1.github.io/post/hugo/hugo-travis-40sec/
- https://github.com/tdewolff/minify/issues/232
- https://stackoverflow.com/questions/47620143/travis-error-the-command-go-get-u-v-github-com-spf13-hugo-failed-and-exited
- https://rileyng.github.io/post/hugo-travis/

---
다음은 Disqus / Google Analytics (Search Engine) 를 연동해볼 것이다.