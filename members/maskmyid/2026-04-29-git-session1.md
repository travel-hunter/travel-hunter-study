# Git Session 1 학습 기록

## 오늘 학습한 내용

### 1. git clone
git clone : Github에 있는 원격 저장소를 내 컴퓨터로 복제하는 명령어

이번 실습에서 'travel-hunter-study' 저장소를 내 컴퓨터로 가져옴.
사용한 명령어: get clone https://github.com/travel-hunter/travel-hunter-study.git

### 2. cd와 경로 이동

cd는 Change Directory의 줄임말로, 현재 작업 위치를 다른 폴더로 이동하는 명령어이다.
이번 실습에서 헷갈렸던 부분은 상대경로와 절대경로였다.
상대경로는 현재 위치를 기준으로 이동하는 방식이다.
예시:
cd .\travel-hunter-study

이 명령어는 현재 위치 안에 travel-hunter-study 폴더가 있을 때만 동작한다.
현재 위치가 C:\PARA\for git 이라면 정상적으로 이동할 수 있다.
하지만 현재 위치가 C:\ 라면 .\travel-hunter-study는 C:\travel-hunter-study를 찾기 때문에 오류가 난다.
절대경로는 현재 위치와 상관없이 전체 경로를 직접 입력하는 방식이다.

예시:
cd "C:\PARA\for git\travel-hunter-study"
또한 폴더 이름에 공백이 있으면 큰따옴표로 감싸야 한다.
예를 들어 for git 폴더는 중간에 공백이 있으므로 아래처럼 입력해야 한다.

cd "C:\PARA\for git"
이번 실습을 통해 .\ 는 현재 위치를 뜻하고, .. 은 상위 폴더를 뜻한다는 것도 알게 되었다


### 3. git status
git status: 현재 git 저장소의 상태 확인 명령어.
현재 브랜치 확인, 수정된 파일 유무, 커밋할 내용 유무 확인 가능.

### 4. git branch
git branch: 현재 로컬 저장소에 있는 브랜치 목록 확인
이번 실습에서 브랜치는 main에 바로 작업하지 않고, 개인 작업 공간을 따로 만들어 안전하게 수정하기 위해 사용.


### 5. git pull
git pull : Github 원격 저장소의 최신 내용을 내 컴퓨터로 당겨서 가져오는(pull) 명령어.
이번 실습에서 main 브랜치의 최신 내용을 가져옴.
git pull origin main을 사용해서 최신 상태라는 메세지를 받았다.

### 6. git switch -c
git switch: 브랜치를 이동하는 명령어.
-c 옵션은 create 줄임말로  git switch를 통해 브랜치를 이동하면서 동시에 새 브랜치를 만들어서 이동했다.
git switch -c study/maskmyid-day1 을 통해 생성하면서 브랜치가 바뀌었다.


### 7. Markdown (.md) 파일 작성
이전에는 members/maskmyid 폴더 안에 Github 웹사이트에서 직접 파일을 만들었는데
이번에는 VS Code 내장 터미널에서 PowerShell 명령어인 New-Item을 사용해 Markdown 파일을 생성했다.

사용한 명령어:
New-Item -ItemType File 2026-04-29-git-session1.md

### 8. git add
git add: 변경한 파일을 Staging Area에 올린다. 
스테이징 에어리어는 커밋 단계 이전의 준비해두는 공간이다.

이번 파일을 올릴 때 사용할 명령어:
git add members\maskmyid\2026-04-29-git-session1.md

### 9. git commit
git commit : 스테이징 단계 이후 올라간 변경 사항을 하나의 기록으로 저장한다.
commit message는 어떤 작업을 했는지 남기는 메모다.

이번 실습에서 사용할 명령어:
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"


### 9. git push
git push: 내 컴퓨터에 저장된 커밋을 Github 원격 저장소로 업로드하는 명령어

이번 실습에서 사용할 명령어:
git push -u origin study/maskmyid-day1

-u 옵션은 현재 로컬 브랜치와 GitHub 원격 브랜치를 연결해주는 역할을 한다.


## 정리

Git의 기본 흐름은 다음과 같다.

작업 폴더에서 파일 작성 또는 수정
-> git status로 상태 확인
-> git add로 staging area에 올림
-> git commit으로 변경 이력 저장
-> git push로 GitHub에 업로드
-> GitHub에서 PR(Pull Request) 생성