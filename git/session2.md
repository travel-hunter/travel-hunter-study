# Session 2: 브랜치와 PR 실습

> 이번 세션의 목표는 `main`을 최신화하고, 새 브랜치를 만든 뒤, 로컬에서 커밋과 push를 해서 PR까지 올려보는 것입니다.

## 0. 시작 전

이 문서는 **Session 1을 끝낸 뒤 이어서 진행하는 문서**입니다.

- 이미 내 컴퓨터에 `travel-hunter-study` 저장소가 clone되어 있다고 가정합니다.
- 여기서 말하는 `travel-hunter-study` 폴더는 각자 자기 컴퓨터에 clone한 폴더입니다.
- 사람마다 저장 위치는 다를 수 있습니다.
- 새 빈 폴더를 만들라는 뜻이 아닙니다.

먼저 확인할 것:

1. Session 1에서 사용한 `travel-hunter-study` 폴더가 내 컴퓨터에 있는지
2. 그 폴더를 VSCode에서 다시 열었는지
3. Git Bash 또는 VSCode 터미널을 열 수 있는지

아직 `travel-hunter-study`가 없거나 Session 1을 하지 않았다면:

- 먼저 [session1.md](./session1.md)를 보고 `git clone`부터 진행합니다.
- Session 2는 Session 1이 끝난 뒤 시작하는 것이 맞습니다.

## 1. 제출 규격

이번 세션 제출물은 아래 3개입니다.

- Markdown 파일 1개
- 브랜치 1개
- PR 1개

제출 파일 경로:

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md
```

브랜치 이름:

```text
study/<아이디>-day2
```

PR 제목:

```text
docs: [git-session2] 브랜치와 PR 실습
```

## 2. 전체 흐름

이번 실습은 아래 순서로 진행합니다.

1. `travel-hunter-study` 폴더를 연다.
2. `main`을 최신화한다.
3. 새 브랜치를 만든다.
4. 제출 문서를 만든다.
5. `git status`로 상태를 확인한다.
6. `git add`로 파일을 스테이징한다.
7. `git commit`으로 기록을 남긴다.
8. `git push`로 GitHub에 올린다.
9. GitHub에서 PR을 만든다.

## 3. 제출 문서에는 무엇을 적나요?

터미널 화면 전체를 그대로 붙일 필요는 없습니다.  
아래 3가지만 정리하면 충분합니다.

- 내가 입력한 명령어
- 화면에 나온 핵심 결과
- 내가 이해한 한 줄

예:

````md
## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
```

## 실행 결과

```text
Already on 'main'
Already up to date.
```

## 내가 이해한 것

- `git switch main`은 main 브랜치로 이동하는 명령어다.
- `git pull origin main`은 최신 main 내용을 내 컴퓨터로 가져오는 명령어다.
````

헷갈리면 뜻을 길게 쓰려고 하지 말고, **명령어 + 결과 한 줄 + 내 말 한 줄**만 적어도 됩니다.

## 4. 실습 순서

### 4-1. `travel-hunter-study` 폴더 열기

Session 1 때 사용했던 같은 `travel-hunter-study` 폴더를 VSCode에서 다시 엽니다.

아직 이 폴더가 없다면 Session 2를 바로 시작하지 말고, 먼저 [session1.md](./session1.md)를 따라 `git clone`부터 진행합니다.

그다음 VSCode 상단 메뉴에서 `터미널 -> 새 터미널`을 누릅니다.

### 4-2. `main` 최신화

터미널에서 아래 명령어를 순서대로 실행합니다.

```bash
git switch main
git pull origin main
```

> `git switch main`은 현재 작업 브랜치를 main으로 이동합니다.  
> `git pull origin main`은 GitHub의 최신 main 내용을 내 컴퓨터로 가져옵니다.

문서에는 아래를 적습니다.

- `main`으로 이동했는지
- `pull` 결과가 어땠는지
- `git pull`이 무엇을 하는지 한 줄 설명

### 4-3. 새 브랜치 만들기

터미널에서 아래 명령어를 실행합니다.

```bash
git switch -c study/<아이디>-day2
```

> `git switch -c`는 새 브랜치를 만들면서 동시에 그 브랜치로 이동합니다. `-c`는 create(생성)의 줄임말입니다.

예:

```bash
git switch -c study/kimjihoon-day2
```

`git switch`가 안 되면 아래 명령어를 사용해도 됩니다.

```bash
git checkout -b study/<아이디>-day2
```

확인할 것:
- 현재 브랜치가 `study/<아이디>-day2`로 바뀌었는지

문서에는 아래를 적습니다.

- 어떤 브랜치를 만들었는지
- 왜 `main`이 아니라 새 브랜치에서 작업하는지 한 줄 설명

### 4-4. 제출 문서 만들기

VSCode에서 아래 경로로 파일을 만듭니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md
```

`members/<본인GitHub아이디>/` 폴더가 없으면 먼저 만들고, 그 안에 파일을 생성합니다.

파일을 만든 뒤 Section 5의 예시를 참고해 내용을 채우고 저장합니다.

### 4-5. 상태 확인

```bash
git status
```

> `git status`는 현재 어떤 파일이 변경되었는지, 스테이징 상태는 어떤지 한눈에 보여줍니다.

문서에는 아래를 적습니다.

- 새 파일이 보였는지
- 현재 브랜치가 무엇인지
- `git status`가 현재 변경 상태를 확인하는 명령어라는 점

### 4-6. `git add` 해보기

```bash
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md
```

> `git add`는 커밋에 포함할 파일을 선택해 스테이징 영역에 올립니다. 아직 저장된 것은 아니고 "커밋 준비 완료" 상태로 만드는 단계입니다.

문서에는 아래를 적습니다.

- 입력한 `git add` 명령어
- `git add` 전후에 `git status` 결과가 어떻게 달라졌는지
- `git add`가 무엇을 하는지 한 줄 설명

### 4-7. `git commit` 해보기

```bash
git commit -m "docs: [git-session2] 브랜치와 PR 실습"
```

> `git commit`은 스테이징된 파일을 하나의 기록(스냅샷)으로 저장합니다. `-m` 뒤의 따옴표 안이 커밋 메시지입니다.

문서에는 아래를 적습니다.

- 입력한 `git commit` 명령어
- 화면에 나온 커밋 결과 한 줄
- `git commit`이 무엇을 하는지 한 줄 설명

### 4-8. `git push` 해보기

```bash
git push -u origin study/<아이디>-day2
```

> `git push`는 내 컴퓨터의 커밋을 GitHub에 올립니다. `-u origin`은 이 브랜치를 GitHub 원격 저장소와 연결한다는 뜻으로, 처음 push할 때 한 번만 씁니다.

예:

```bash
git push -u origin study/kimjihoon-day2
```

문서에는 아래를 적습니다.

- 입력한 `git push` 명령어
- 화면에 나온 핵심 결과 한 줄
- `git push`가 무엇을 하는지 한 줄 설명

> 처음 push할 때 로그인이나 토큰 입력이 필요할 수 있습니다. GitHub 비밀번호 대신 PAT를 사용할 수 있습니다.

### 4-9. GitHub에서 PR 만들기

1. GitHub에서 `travel-hunter-study` 저장소를 엽니다.
2. `Compare & pull request` 버튼이 보이면 클릭합니다.
3. 보이지 않으면 `Pull requests -> New pull request`로 들어갑니다.
4. PR 제목을 아래처럼 적습니다.

```text
docs: [git-session2] 브랜치와 PR 실습
```

5. PR을 생성합니다.

문서에는 아래를 적습니다.

- 브랜치가 왜 필요한지 한 줄 설명
- PR이 어떤 역할을 하는지 한 줄 설명

## 5. 제출 문서 예시

아래는 완성된 제출 문서의 예시입니다. 내 문서와 비교해 빠진 항목이 없는지 확인하세요.

````md
# Git 기초 학습 Session 2 (YYYY-MM-DD)

## 오늘 배운 개념
- `git pull`은 원격 저장소의 최신 내용을 내 컴퓨터로 가져오는 명령어다.
- 브랜치는 main을 직접 건드리지 않고 따로 작업하는 공간이다.
- `git push`는 내 브랜치의 커밋을 GitHub에 올리는 단계다.
- PR은 내 브랜치 작업을 main에 합쳐달라고 요청하는 단계다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/kimjihoon-day2
git status
git add members/kimjihoon/YYYY-MM-DD-git-session2.md
git commit -m "docs: [git-session2] 브랜치와 PR 실습"
git push -u origin study/kimjihoon-day2
```

## 실행 결과

```text
Already on 'main'
Already up to date.
Switched to a new branch 'study/kimjihoon-day2'
[study/kimjihoon-day2 abc1234] docs: [git-session2] 브랜치와 PR 실습
branch 'study/kimjihoon-day2' set up to track 'origin/study/kimjihoon-day2'
```

## 직접 해본 것
- `main`을 최신 상태로 맞췄다.
- 새 브랜치를 만들어 그 안에서 작업했다.
- 로컬에서 커밋한 뒤 GitHub에 push했다.
- PR을 생성했다.

## 헷갈렸던 점
- `main`에서 바로 작업하면 안 되는 이유를 더 익히고 싶다.

## 다음 학습 예정
- 다음 세션에서 충돌 해결과 merge 흐름을 더 익힐 예정이다.
````

## 6. 완료 체크리스트

아래 항목을 모두 만족하면 완료입니다.

- `members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md` 파일을 만들었다.
- `git switch main`과 `git pull origin main`을 실행했다.
- `study/<아이디>-day2` 브랜치를 만들었다.
- `git status`를 실행했다.
- `git add`를 실행했다.
- `git commit`을 실행했다.
- `git push`를 실행했다.
- 브랜치와 PR의 역할을 문장으로 적었다.
- 실습 내용을 `.md` 문서에 정리했다.
- GitHub에 브랜치를 올렸다.
- PR을 생성했다.
