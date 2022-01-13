# Git이란?

- 분산 버전 관리 시스템



### 버전 관리란?

- 코드의 **히스토리(버전)**을 관리하는 도구
- 개발되어 온 **과정** 파악 가능
- 이전 버전과의 **변경사항 비교 및 분석**
- ex) A라는 파일이 '각'이라는 글자라면 버전관리를 할 경우 'ㄱ' -> '가' -> '각' 과정을 파악 가능
- 얼마든지 이전 버전으로 돌아가는 것이 가능해짐



### 분산이란?

- 반의어: 중앙 집중
- Git은 **변경 사항만**을 저장함
  - ex) A파일이 '갑'이라고 하면, ver1에는 'ㄱ', ver2에는 'ㅏ', ver3에는 'ㅂ'이 저장됨
- 각 버전은 Version Database에 저장되고 이러한 Database는 VCS Server/Server Computer에 저장됨
- 즉, Version 관리를 하는 프로그램이 Git이고, Git으로 Version관리한 것을 저장하는 저장소가 GitHub
- GitLab, GitHub, Bitbucket : Git을 이용하여 버전 관리를 한 데이터의 저장소 역할
  - GitHub는 MS소유. 즉 Git을 활용하고 GitHub에 저장하면 사실상 MS의 데이터베이스에 들어가있는 것
  - 그래서 GitLab은 그 서버 자체를 내가 구축할 수 있게 해줌. 내 컴퓨터에 저장소를 구현할 수 있도록. SSAFY가 GitLab를 사용하는 이유가 여기에 있음
- 즉 Git과 GitHub는 **다르다**



# cmd와 powershell

### cmd vs powershell

- cmd에서는 unix 명령어가 안됨
- unix, linux, windows 중에서 unix와 linux는 같은 계열
  - 개발자는 mac을 써야한다는 말이 있는 이유는 mac을 쓰면 unix와 linux의 특성을 이용해서 개발하기 편리하게 되어있기 때문
  - 앞으로 개발을 하면서 unix, linux 명령어를 많이 쓸 것.
- CLI(Command Line Interface): 명령어를 이용해서 인터페이스를 조작하는 환경
- CLI와 대비되는 환경은 GUI(Graffic User Interface)
- 아무튼 이런 unix, linux 명령어가 cmd에서는 지원되지 않음
- 그래서 ms가 내놓은게 powershell. 여기서는 **일부** unix, linux 명령어가 동작



### unix/linux 명령어

- ls: 파일의 위치와 파일 목록을 보여주는 명령어. cmd에서는 dir로 써야함
- clear: 화면을 지우고 싶을때. cmd에서는 cls
- cd<path> : 현재 위치 이동하기   ex) cd Music
- cd .. : 한칸 상위 폴더로 이동
- mkdir<name>: 폴더 생성하기   ex) mkdir ssafy7
- touch<name>: 파일 생성하기  
  - powershell에서는 지원 안함
  - gitbash 이용
    - ~(물결표시)는 user의 home 디렉토리를 의미. home 디렉토리는 cmd나 powershell을 처음 열었을때 표시되는 위치임
- rm <name>: 파일 삭제   ex) rm test.txt
- rm -r <name> : 디렉토리 삭제하기   ex) rm -r test
  - 여기서 -r은 recursive하게 지운다는 의미를 포함.
  - 즉 test라는 폴더를 지울 때 그 안에 포함된 것들을 한꺼번에 지운다는 의미
- 탭: 명령어를 자동 완성해주는 키.   ex) cd ss 까지만 치고 탭을 누르면 ssafy7이 뜬다. ss로 시작하는 폴더가 여러개라면 탭을 누를때마다 순서대로 보여줌

---



# VS Code

### 실행

- 실행하고 싶은 위치에서 우클릭 후 Code로 열기
- 그냥 여는 것과의 차이는: 아무런 위치도 선택되지 않은 상태로 실행됨. code로 열기는 자동으로 위치를 지정해서 실행해주니까 조금 더 편리
- gitbash에서 "code ." 입력하면 현재 위치에서 vs code가 실행됨



### RacingGround 프로젝트 생성하기

- mkdir RacingGround
- touch BasicCar.py
- README.md  >> 프로젝트 설명서
- Repository: 특정 디렉토리를 버전 관리하는 저장소
  - git init: 로컬 저장소를 생성하는 명령어. 
  - .git 디렉토리에 버전 관리에 필요한 모든 것이 들어있음
- 우리는 ssafy7 폴더가 아니라 RacingGround 폴더에 git init을 해주자.
  - 그러면 vscode에서 RacingGround 이하 파일들이 초록색으로 변하는데, 이는 버전관리대상이라는 것을 의미함
  - .git은 자동으로 생성되는데, 이 존재로 이곳이 Repository라는 것을 인식할 수 있게됨
  - 디렉토리명에서 .으로 시작하면 숨김파일 처리가 돼있음
- 현재 상태를 **특정버전**으로 남긴다 = **"커밋(Commit)"** 한다
  - 즉 우리의 repository안에 commit이 점점 쌓여가는 것



### Commit

- Commit은 3가지 영역을 바탕으로 동작

  - Working Directory: 내가 작업하고 있는 실제 디렉토리 
    - untracked 상태: git으로부터 추적되고 있지 않은 상태. 우리의 경우 basic, readme
    - untracked인 상태에서 명령어 `git add`를 해주면 git으로부터 tracked인 상태가 됨
    - `git add`를 해주면 달라진 상태를 staging area에 올려줌(staged된 상태)
    - 그 상태에서 `git commit`을 해주면 commit된 상태가 됨
  - Staging Area: 커밋으로 남기고 싶은, 특정버전으로 관리하고 싶은 파일이 있는 곳/잠시 위치하는 곳. 실제로 존재하지는 않지만 git이 생각하는 가상 공간
  - Repository: 커밋들이 저장되는 곳 >> .git

- 예를 들어 Version2가 "싸피" 이고 여기에 !를 추가해서 Version3를 "싸피!"라고 하면, "싸피!"가 저장되는 곳이 Repository

- 그리고 Staging Area에는 Ver2 >> Ver3의 변경 사항인 "!"가 *잠시* 저장됨

- `git status`: 현재 git으로 관리되고 있는 파일들의 상태를 알 수 있는 명령어

  - No commits yet

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
            BasicCar.py
            README.md

    nothing added to commit but untracked files present (use "git add" to track)

- `git add`

  - No commits yet

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)
            new file:   BasicCar.py
            new file:   README.md

- `git commit -m "commit_message"`: commit을 하는데, 메세지를 남김(-m). 

  - commit message는 구체적으로 남기는 게 좋음(어떤 작업을 했는지)

- 이렇게 commit을 남기고 다시 작업을 하게되면, git이 이미 이 파일을 버전관리하고 있는데 수정된(modified) 상태가 됨. 이후 같은 과정을 반복하면서 commit01, commit02, commit03...이 계속 생성됨

- `git add .`(쩜 찍어야됨): **추적되지 않은 모든 파일**과, 추적하고 있는 파일 중 **수정 된 파일**을 **"모두"** Staging Area에 올림

  - .에는 현재위치라는 의미가 있음. cd .. 에서 ..에는 부모위치 의미가 있다는 것을 생각하면 좋음

- `git log`: git log를 쳤을 때 commit 옆에 뜨는 것은 고유한 커밋아이디

- A폴더에서 git init을 하면 그 하위 폴더 전부 관리 대상이 됨. 따라서 하나의 repository 안에 새로운 repository를 생성하면 X. 실수했을 경우 숨김파일 보기를 해서 .git을 지우면 됨.

- `git log`: git의 commit history 보기
- `git diff`: 두 commit 간 차이 보기
  - 고유 id에서 같은 repo 내에서는 앞 4자리만 입력해도 됨. ex) git diff be80 d0a8
  - id의 순서를 바꾸면 출력도 달라짐
  - 앞의 것을 기준으로 뒤의 것을 비교해줌



# Git Commit 실습

- 바탕화면에 edu_git_commit 폴더를 만들고 git 저장소를 생성

- 해당 폴더 안에 a.txt라는 텍스트 파일을 만들고, add.a.txt라는 커밋메세지로 커밋을 만들어주세요

- 이번에는 b.txt라는 텍스트 파일을 만들고, add b.txt라는 커밋메세지로 커밋을 만들어주세요

- a.txt 파일을 수정하고 "update a.txt"라는 커밋메세지로 커밋을 만들어주세요

- 풀이)

  - mkdir edu_git_commit

  - cd edu_git_commit

  - git init

  - touch a.txt

  - git add a.txt

  - git commit -m "add a.txt" -> 누구냐고 물어볼거임

  - git config user.name ""

  - git config user.email ""

  - git commit -m "add a.txt"

  - ---

  - touch b.txt

  - git add .

  - git commit -m "add b.txt"

  - ---

  - vs code에서 수정하면 왼쪽 목록에 "M" 표시가 뜰 것임

  - 그 후 git commit -m "update a.txt"

### 

#### Q. 왜 굳이 stage area를 거쳐야 하죠?

- 커밋으로 "남기고 싶은", 특정 버전으로 "관리하고 싶은" 

---

### 실습 이어서

- BasicCar.py에서 코드 수정
- 코드 수정하면 BasicCar.py에 생기는 흰 동그라미는 저장이 안되었다는 뜻



---

### Remote Repository 연결

- github repository 생성
  - public은 모두에게 공개, 소스코드를 누구나 가져갈 수 있음. 하지만 다른 사람이 내 repo에 올릴 수는 없음
  - private은 3가지 전부 안됨
- branch? main? master?
  - main: 깃허브에서 만들어지는 메인 스트림의 이름
- `git clone`
- git hub에다가 만든 test_repo는 remote, test_repo를 local로 가져온 게 clone. 일단 가져오고나면 그 이후에는 변경사항만 주고받으면 됨(push & pull)
- git add할 때는 신원이 필요 x, git commit에는 필요
- `git clone` >> `git push` 
- github 페이지에서도 직접 변경 가능. 오른쪽 연필 모양 누르고 commit change 누르면 됨. 
  - 이렇게 되면 remote와 local 중 remote가 최신인 상태가 되는데, 이 상황에서 해야 하는 것이 `git pull`
  - 그 후 code . 를 입력해 파일을 보자
- 정리하면, local 과 remote 사이
  - 실습에서는 임의로 상황을 만들어서 github에서 직접 수정을 했는데, 사실 이런 상황은 거의없음.
  - 실제로는 여러명의 개발자가 remote에서 clone을 해서 각자 local에서 작업을 하고 push하는 형태. 작업하기 전에 다른 작업자가 해놓은 작업을 pull 해야함.

---

- 빈 repository를 하나 만들어야 함
- github에서 new repository 생성(RacingGround)
  - 그러면 원격저장소에 아무것도 없는 repository를 하나 만든 것임

- 먼저 git status를 쳐서 상태를 보자. nothing to commit, working tree clean 떠야 함.
- git remote add origin https://github.com/Ryu4949/RacingGround.git
  - 뒷부분이 remote repository 주소인데, 너무 기니까 앞으로 이 주소를 "origin"이라고 부를거야 라는 의미	
  - 여기서 origin은 보통 remote repository에 **관례적**으로 붙이는 이름
- git push -u origin master
  - -u 옵션은 지금처럼 remote repository를 등록한 직후에만 쓰면 됨
  - -u는 set upstream의 약자
  - 지금 우리는 local에 master라는 큰 줄기를 갖고 있는데, 얘를 remote로 보내는 과정이라고 생각하면 됨. branch의 존재를 remote에 알려주는 것
  - 즉 위 명령어의 의미는 'master' branch(history tree)를 origin에 보내겠다는 의미
  - github에 racingground에 들어가보면 왼쪽 윗부분에 master가 있음을 알 수 있을 것이다
- `git clone {remote_repo}`: remote repo를 local로 복사
- `git push origin master`: local repo의 최신 커밋을 remote repo로 push함

​	

---

### repository 삭제

- github >> repositories >> repo 선택 >> setting >> 맨 아래 danger zone >> delete this repository >> 주어진 내용 입력 >> ...
