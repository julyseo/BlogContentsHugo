---
title: "[Dev] Git 그냥 일단 시작해보기 (GitHub)"
date: 2019-06-05T17:15:54+09:00
categories:
- Development
- Git
tags:
- Development
- Tips
- Git
- GitHub
- Usage
- 깃
- 깃허브
- 사용법
- 개발
- 팁
#thumbnailImage: //example.com/image.jpg
---
본 글에서는 Mac 에서 Git 을 설치 및 사용하는 방법을 설명한다.  
원격 저장소로는 Github 를 사용한다.
<!--more-->

<!--toc-->

# 들어가기 전에 
Git 의 사용은 개발자라면 기본 중에 가장 기본이 되는 부분이다.

기본만 알고도 사용하는데에는 무리가 없으나 파고들수록 심화되는 내용도 많고,  
그 내용을 바탕으로 생산성이 기하급수적으로 증가하기 때문에  
나도 원래는 심화 내용을 공부하고, 정리하는 글을 쓰려고 했다.

그러나 맨땅에 헤딩식으로 배운탓에 기본적으로 알고 있어야 될 내용도  
모르고 자주 사용하는 기능만 쓸 줄 알았기 때문에   
기본을 다지는 것부터 시작해야겠다는 생각이 들었다.

물론 이번 글에서 다룰 내용은 쓰다보면 아는 것을 넘어 익숙해지기 때문에  
딱히 여러번 다시 볼 내용은 아니지만,  
내가 git을 처음 쓸 때 방향을 잃고 헤매던 삐약이 시절이 있었고  
앞으로도 여러 사람이 그렇게 삽질을 할 것이기 때문에  
조금이나마 길잡이 역할이 되는 글을 쓰려고 한다.

개인적으로 이론이나 흐름을 이해하는 과정없이 "일단 해보세요" 하는 설명을 가장 싫어하는데,  
내가 git 을 그렇게 배우기도 했고, 기능 특성상 일단 해보고 이해하는게 가장 효율적이라고 생각되어  
이번 글에서는 Git 의 기본적인 사용법을 "일단 해보는 식" 으로 살펴볼 것이다.  
*백문이 불여일견*

# 0. Git?
Git을 줄이고 줄여 한마디로 설명하자면 코드 관리하는 툴이다.  
그러나 Github 같은 서비스가 생겨나면서 부터는 한마디로 정의하기가 어려워졌다.

Git 은 CLI 기반으로, 터미널에서 명령을 입력하면 동작하는 프로그램이다.  
요새는 GUI 로 제공하는 어플리케이션도 많이 등장을 하였지만,  
CLI 를 기반으로 일부 기능을 사용하기 쉽게 UI 로 감싸 제공하는 식이라서 모든 기능을 제공하지는 않는다.  

> 모든 기능을 제공할 수도 있다. **앱마다 제공하는 기능이나 UI는 천차만별이다**

대표적인 기능은 변경사항을 누가 언제 어느 부분을 수정했는지에 대한 히스토리 관리 *(commit)*부터,  
협업자간의 코드 공유 및 코드 통합 (clone, merge),  
그리고 의존하는 모듈도 Git 으로 관리할 수 있다. (submodule)  

> 대표적인 것들만 써놓은 것이고, 실제로는 더 다양한 기능들이 있다

그 외 GitHub/GitLab 같은 서비스에서는  
프로젝트와 CI tool 과 연동하여 빌드, 테스트, 배포를 자동화할 수 있으며  
코드 리뷰, 이슈 트래킹와 같이 협업을 도와주는 기능을 제공하며  
**무엇보다 git 의 기본 기능과 위의 것들을 웹에서, 잘감싸진 GUI 형태로 제공한다.**  

> 물론 모든 git 의 기능을 다 다룰 수 있는 건 아니지만  
> 원격 저장소로써 사용할만한 기능은 거의 전부 제공한다.

# 1. Git 설치
brew 로 설치하거나 커맨드 라인 도구를 설치하여 git 을 사용할 수 있다.

## A. brew 를 사용하여 설치
brew 는 아래 명령을 입력하여 설치할 수 있다.  
**`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`**

그리고 **`brew install git`** 명령을 입력하여 git 을 설치한다.

**`git --version`** 명령을 입력하여 버전 출력여부로 설치된 것을 확인할 수 있다.
![](/res/img/dev/git_normal_version.png) *바이너리는 `/usr/local/bin` 내에 설치된다.*

## B. 커맨드 라인 도구(Command Line Tools)를 설치하여 사용

터미널에서 다음과 같이 입력하여 커맨드 라인 도구를 설치한다.  
**`xcode-select --install`**  
명령어를 입력하면 몇가지 창이 출력될텐데, 동의 및 설치한다.

그리고 **`git --version`** 명령을 입력하여 버전 출력여부를 확인한다.  
![](/res/img/dev/git_apple_version.png) *바이너리는 `/usr/bin` 내에 설치된다.*

> brew 로 설치한 git 과 버전이 다를 수 있으며,  
> brew 와 커맨드 라인 도구 두 버전을 동시에 설치할 수도 있다. (바이너리 위치가 다름)

# 2. GitHub 가입 및 초기설정
git 을 사용하기 위해 유저 정보를 설정해주어야 하는데,  
보통 이 정보들은 GitHub 계정의 정보와 동일하게 설정해준다.

아직 [GitHub](https://github.com)에 가입하지 않았다면 접속하여 가입해준다.

그리고 아래 명령을 사용하여 이름과 이메일 정보를 설정해준다.  
`git config --global user.name <NAME>`  
`git config --global user.email <EMAIL>`

이렇게하면 git 을 사용할 준비는 끝났다.

> GitHub 가 아니라 GitLab, BitBucket 등으로 가입하여 사용해도 상관없다.  
> 서비스별로 제공하는 기능이나 과금 정책등이 다르기 때문에 참고하여 선택하면 된다.  
> 여기서는 편의 상 가장 많이 알려진 GitHub 를 사용한다.

# 3. 저장소 생성하기
저장소를 생성하고 간단한 변경사항을 만든뒤 커밋하는 튜토리얼을 진행할 것이다.  
로컬/원격에서 각각 생성할 수 있으며 생성하는 방법이 약간 다르다.

## 3-1. 로컬 저장소 생성 및 커밋 (Local Repository) : git init
로컬에서 명령을 입력하여 저장소를 생성한다.

1. 폴더 생성하고 터미널에서 `git init` 명령 입력  
![](/res/img/dev/gitinit.png)  
저장소의 상태 확인은 `git status`를 입력하여 확인한다.  
![](/res/img/dev/gitstatus.png)  
제대로 생성되지 않았다면 아래와 같이 출력된다.  
![](/res/img/dev/git_status_norepo.png)

2. 변경사항을 만들기 위해 `touch README.md` 를 입력한다.  
![](/res/img/dev/gitstatuschanged.png)

3. `git add README.md` 를 실행시켜 생성한 README.md 파일을 커밋할 변경 사항에 포함한다.  
![](/res/img/dev/gitadded.png)

4. `git commit -m "First Commit"` 을 실행시켜 "First Commit" 메세지를 포함한 커밋을 생성한다.
![](/res/img/dev/firstcommit.png)  
커밋 히스토리 조회는 `git log`를 입력하여 확인한다.
![](/res/img/dev/gitlog.png) *q 키를 눌러 빠져나온다*

커밋을 함으로써 revision 을 생성했다. 

> 이후 `git remote add` 명령을 입력하여 원격 저장소와 연동할 수도 있다.

## 3-2. 원격 저장소 생성 및 복제 (Remote Repository) : git clone
GitHub 페이지에서 저장소를 만들고 복제해온다.

1. [GitHub](https://github.com)에 접속하여 로그인한다.

2. 우측 상단의 +버튼을 눌러 new repository 를 선택한다.  
![](/res/img/dev/github_new_repo.png)

3. 저장소 관련 정보를 설정하고 Create repository 를 누른다.  
![](/res/img/dev/github_create_repo.png)

4. 저장소가 생성된 것을 확인하고 주소를 복사한다.  
![](/res/img/dev/github_repo_page.png) *.git 으로 끝나는 주소를 복사* 

5. `git clone` 명령으로 복사된 주소로 복제한다.  
![](/res/img/dev/gitclone.png) *warning 은 가볍게 무시*

6. 변경사항을 만들고 커밋한다. (로컬 2~4 과정)
![](/res/img/dev/gitcommit_sum.png)

7. `git push` 명령으로 로컬 저장소의 변경사항을 원격 저장소에 저장한다.
![](/res/img/dev/gitpush.png) *private 저장소인 경우 아이디/비밀번호를 물어볼 수 있다.*

8. GitHub 페이지에서 제대로 올라갔는지 확인할 수 있다.
![](/res/img/dev/github_result.png)

> 원격 저장소는 로컬 저장소와는 다른 구조를 가지고 있다.  
> 어떤 부분이 다른지는 다음 글에서 설명할 예정이다.

---
어려운 용어가 난무하는 것 같지만 막상 쓰다보면 익숙해질 것들이다.

내부 구조나 이론적인 것들을 이해할 필요없이,   
명령들이 어떤 걸 의미하는지만 알면 git 을 충분히 활용할 수 있다.    
그러니 일단 `init, add, commit, clone, push` 명령을 사용하는 것에 익숙해지자.