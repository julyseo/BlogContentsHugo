



# Mac Git 기본 사용법


Mac 에서 Git 을 설치하고  
*로컬 저장소 생성하기* 와 *GitHub를 이용하여 원격 저장소 생성 및 불러오기*  
이 두 가지를 진행해볼 것이다.

> 생소한 용어와 명령어가 난무할 예정이지만 여기서는 해보는 것에 초점을 맞춘다.  
> 관련 용어나 명령어는 뒤에서 설명할 것이다

#### Git 설치하기

GUI 클라이언트도 있지만 명령어에 익숙해지기 위해 CLI 기반 Git 클라이언트를 설치한다.

XCode 의 커맨드 라인 툴을 설치하면 Git 이 기본적으로 포함되어 있다.  
기본 Git 외의 다른 버전을 설치하여 사용하고 싶으면 brew 를 이용하면 된다.

`brew install git`

설치를 마치고나면 user 정보를 설정한다.  
name 과 email 을 다음과 같이 설정해준다.

`git config --global user.name "NAME"`  
`git config --global user.email "EMAIL"`

지금은 임의의 name 과 email 을 넣어도 상관없다.  
이렇게해서 터미널에서 git 을 사용할 준비를 마쳤다.

이제 저장소를 생성하고 간단하게 사용해볼 것인데,  
로컬 저장소만으로 관리를 할지, 원격으로 관리할 지에 따라 시작하는 방법이 조금 다르다.  
물론 로컬 저장소로 시작하여 나중에 원격 저장소에 연동할 수도 있기는 하지만 조금 더 손이 많이간다.

#### 로컬 저장소 생성하기
여기에서는 로컬 저장소를 생성하고 간단한 수정 사항을 만들어 커밋을 해볼 것이다.

##### 저장소 생성
로컬 저장소는 `git init` 명령으로 생성한다.  
아무 폴더나 만들어 터미널로 이동하여 `git init` 을 실행하면 아래와 같이 나온다.  
![](/res/img/dev/gitinit.png)  
그리고 `git status` 를 실행해보면 아래와 같이 출력된다.  
![](/res/img/dev/gitstatus.png) *master 라는 브랜치로 저장소가 만들어졌고, 아무 수정사항이 없어 커밋할 내용이 없다는 뜻이다.*

##### 커밋에 반영할 변경사항 선택
변경사항을 만들기 위해 `touch README.md` 를 실행한다. (빈 파일을 만드는 명령어이다) 
![](/res/img/dev/gitstatuschanged.png) *추적하지 않는 파일이 생겼다.*  
`git add README.md` 를 실행시켜보면 아래와 같이 출력된다.  
![](/res/img/dev/gitadded.png) *이제 커밋할 변경 사항이 만들어졌다.*

> 이 상태를 **Staged** 라고 표현한다. (스테이지 해제는 Unstage 라고 함)

##### 변경사항 반영
마지막으로 `git commit -m "First Commit"` 을 실행하여 Local Repository 에 변경사항을 반영한다.  
![](/res/img/dev/firstcommit.png) *커밋 성공*  
이번에는 `git log`로 확인해본다.
![](/res/img/dev/gitlog.png) *이상한 문자열과 브랜치 상태, 유저 정보, 날짜, 그리고 커밋 메세지가 출력된다. (q를 입력하여 빠져나간다)*

이렇게 **커밋을 함으로써 revision 이 생성**되었다.  

*revision* 이란 특정 커밋을 가리키는 표현식인데,  
바로 위에 `git log` 를 통해 봤던 이상한 문자열이다.  
간단하게 각 커밋을 구분하기 위한 고유한 값, 더 쉽게는 버전으로 생각하면 될 것 같다.

#### GitHub를 이용하여 원격 저장소 생성 및 불러오기

원격 저장소는 bare 저장소라고 하며, 로컬 저장소와 다르게 bare 저장소에는 작업 공간이 없다.

Git 서버에서 원격 저장소를 생성한 뒤,  
그 저장소를 복제함으로써 로컬 저장소를 생성한다.

보통은 Git 서버를 직접 구축하지 않고   
Github/Gitlab 등의 서비스를 이용하는데,  
여기서는 GitHub 를 이용할 것이다.

그러기 위해서 [GitHub](https://github.com) 에 가입한다.

그리고 로그인하여 새로운 저장소를 생성한다.

이렇게 하면 원격 저장소가 생성된 것이다.

이제 이 원격 저장소를 복제하여 작업하고 커밋할 것이다.

GitHub 가입할 떄의 이름과 메일 주소로 config 를 다시 설정한다.

`git config --global user.name "GitHub Name"`  
`git config --global user.email "GitHub Email"`

아까 만든 저장소를 복제한다

`git clone https://github.com/<Name>/<Repo Name>`

로컬 저장소가 생성되었다.

여기서부터는 로컬 저장소에서 했던 것처럼 변경사항을 만들고 커밋해볼 것이다.

`touch README.md`
`git add README.md`
`git commit -m "First Commit"`

변경 사항을 반영했다.

그리고 GitHub 에 가보면

아무것도 뜨지 않는다.

커밋은 로컬 저장소에만 반영하는 것이고,
이 커밋을 원격 저장소에 반영하기 위해서는 push 해야한다.

`git push`

비밀번호를 입력하면 푸시가 완료되고

GitHub 원격 저장소에도 반영이 된 것을 볼 수 있다.