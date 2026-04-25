# Session 1: Git 기초와 첫 업로드

> 이번 세션의 목표는 제출 문서를 먼저 정리한 뒤, 내 컴퓨터에서 `git add`, `git commit`, `git push`를 실행해 GitHub PR까지 만드는 것입니다.

## 이전 세션과 연결

이 세션은 Git/GitHub 제출 흐름의 시작점입니다. 저장소를 clone하고, 내 브랜치에서 문서를 작성한 뒤 GitHub에 PR로 올리는 전체 흐름을 처음부터 끝까지 경험합니다.

## 0. 시작 전

이 문서에서 말하는 `travel-hunter-study`는 **각자 컴퓨터에 `git clone`으로 내려받은 저장소 폴더**입니다.

- 새 빈 폴더를 임의로 만드는 것이 아닙니다.
- 경로는 사람마다 다를 수 있습니다.
- 중요한 것은 **clone으로 생성된 `travel-hunter-study` 폴더를 VSCode에서 여는 것**입니다.

이미 clone했다면:
- 그 폴더를 VSCode에서 엽니다.

아직 없다면:
1. 원하는 작업 폴더로 이동합니다.
2. 아래 명령어를 실행합니다.

```bash
git clone https://github.com/travel-hunter/travel-hunter-study.git
```

3. 생성된 `travel-hunter-study` 폴더를 VSCode에서 엽니다.

VSCode에서 여는 방법:
1. VSCode 실행
2. `파일 -> 폴더 열기`
3. `travel-hunter-study` 폴더 선택
4. 열기

## 1. 제출 규격

이번 세션 제출물은 아래 3개입니다.

- Markdown 파일 1개
- 브랜치 1개
- PR 1개

제출 파일 경로:

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session1.md
```

브랜치 이름:

```text
study/<아이디>-day1
```

PR 제목:

```text
docs: [git-session1] Git 기초와 첫 업로드
```

## 2. 전체 흐름

이번 실습은 아래 순서로 진행합니다.

1. `travel-hunter-study` 폴더를 연다.
2. 작업 브랜치를 만든다.
3. 제출 문서를 만든다.
4. 제출 문서에 오늘 배운 내용과 실습할 명령어를 먼저 정리한다.
5. `git status`로 파일 상태를 확인한다.
6. 문서를 저장했는지 확인한다.
7. `git add -> git commit -> git push`를 한 번에 진행한다.
8. GitHub에서 PR을 만든다.

## 3. 제출 문서에는 무엇을 적나요?

터미널 화면 전체를 그대로 붙일 필요는 없습니다.
처음에는 아래 내용만 정리해도 충분합니다.

- 오늘 배운 개념
- 오늘 실습할 명령어
- `git status`로 확인한 파일 상태
- 직접 해본 것
- 헷갈렸던 점

예:

````md
## 오늘 배운 개념
- `git add`는 커밋할 파일을 올려두는 단계다.
- `git commit`은 변경 내용을 하나의 기록으로 남기는 단계다.
- `git push`는 내 컴퓨터의 커밋을 GitHub에 올리는 단계다.

## 오늘 실습할 명령어

```bash
git status
git add members/kimjihoon/YYYY-MM-DD-git-session1.md
```

## git status 확인

```text
On branch study/kimjihoon-day1
Untracked files:
```

## 직접 해본 것
- 새 브랜치를 만들었다.
- 제출 문서를 만들었다.

## 헷갈렸던 점

## 다음 학습 예정
- 다음 세션에서 `git pull`, 브랜치, PR 흐름을 더 익힐 예정이다.
````

헷갈리면 뜻을 길게 쓰려고 하지 말고, **내가 이해한 말로 한두 줄**만 적어도 됩니다.

## 4. 실습 순서

### 4-1. `travel-hunter-study` 폴더 열기

이미 clone했다면 그 폴더를 VSCode로 열고, 상단 메뉴에서 `터미널 -> 새 터미널`을 누릅니다.

아직 clone하지 않았다면 원하는 작업 폴더에서 아래 명령어를 실행합니다.

```bash
git clone https://github.com/travel-hunter/travel-hunter-study.git
```

그다음 생성된 `travel-hunter-study` 폴더를 VSCode로 엽니다.

### 4-2. 작업 브랜치 만들기

터미널에서 아래 명령어를 실행합니다.

```bash
git switch -c study/<아이디>-day1
```

> `git switch -c`는 새 브랜치를 만들면서 동시에 그 브랜치로 이동합니다. `-c`는 create(생성)의 줄임말입니다.

예:

```bash
git switch -c study/kimjihoon-day1
```

`git switch`가 안 되면 아래 명령어를 사용해도 됩니다.

```bash
git checkout -b study/<아이디>-day1
```

확인할 것:
- 현재 브랜치가 `study/<아이디>-day1`로 바뀌었는지

### 4-3. 제출 문서 만들기

VSCode에서 아래 경로로 파일을 만듭니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session1.md
```

`members/<본인GitHub아이디>/` 폴더가 없으면 먼저 만들고, 그 안에 파일을 생성합니다.

파일을 만든 뒤 아래 템플릿을 붙여넣고 먼저 채웁니다.

````md
# Git 기초 학습 Session 1 (YYYY-MM-DD)

## 오늘 배운 개념
- `git add`는 커밋할 파일을 올려두는 단계다.
- `git commit`은 변경 내용을 하나의 기록으로 남기는 단계다.
- `git push`는 내 컴퓨터의 커밋을 GitHub에 올리는 단계다.

## 오늘 실습할 명령어

```bash
git switch -c study/kimjihoon-day1
git status
git add members/kimjihoon/YYYY-MM-DD-git-session1.md
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"
git push -u origin study/kimjihoon-day1
```

## git status 확인

```text
아직 실행 전입니다. git status 실행 후 새 파일이 보이는지 한 줄로 정리합니다.
```

## 직접 해본 것
- 새 브랜치를 만들었다.
- 제출 문서를 만들었다.
- `git status`로 파일 상태를 확인했다.
- `git add`, `git commit`, `git push`로 GitHub에 올렸다.

## 헷갈렸던 점
-

## 다음 학습 예정
- 다음 세션에서 `git pull`, 브랜치, PR 흐름을 더 익힐 예정이다.
````

> 이 문서는 먼저 완성하고 저장합니다. `commit`이나 `push` 결과를 반드시 붙여넣을 필요는 없습니다.

### 4-4. 상태 확인

```bash
git status
```

> `git status`는 현재 어떤 파일이 변경되었는지, 스테이징 상태는 어떤지 한눈에 보여줍니다.

확인할 것:
- 현재 브랜치가 `study/<아이디>-day1`인지
- 새로 만든 제출 문서가 보이는지
- `untracked files` 같은 문구가 나오는지

제출 문서의 `git status 확인` 부분에는 새 파일이 보였는지만 한 줄로 적으면 됩니다.

### 4-5. 최종 저장 확인

`git add`를 실행하기 전에 VSCode에서 제출 문서를 저장했는지 확인합니다.

확인할 것:
- 제출 문서 파일명이 `YYYY-MM-DD-git-session1.md`인지
- 파일 위치가 `members/<본인GitHub아이디>/` 아래인지
- 문서 내용에 빈 항목이 너무 많이 남아 있지 않은지

> `git add` 이후 문서를 다시 수정했다면 `git add`를 한 번 더 실행해야 합니다. 처음에는 헷갈리지 않도록 문서를 먼저 저장한 뒤 Git 명령어를 진행합니다.

### 4-6. `git add`로 스테이징

```bash
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session1.md
```

> `git add`는 커밋에 포함할 파일을 선택해 스테이징 영역에 올립니다. 아직 저장된 것은 아니고 "커밋 준비 완료" 상태로 만드는 단계입니다.

`git add` 후 상태를 확인합니다.

```bash
git status
```

확인할 것:
- 제출 문서가 `Changes to be committed` 아래에 보이는지

### 4-7. `git commit`으로 기록 남기기

```bash
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"
```

> `git commit`은 스테이징된 파일을 하나의 기록(스냅샷)으로 저장합니다. `-m` 뒤의 따옴표 안이 커밋 메시지입니다.

커밋 후 상태를 확인합니다.

```bash
git status
```

확인할 것:
- `nothing to commit` 또는 작업트리가 깨끗하다는 문구가 보이는지

### 4-8. `git push`로 GitHub에 올리기

```bash
git push -u origin study/<아이디>-day1
```

> `git push`는 내 컴퓨터의 커밋을 GitHub에 올립니다. `-u origin`은 이 브랜치를 GitHub 원격 저장소와 연결한다는 뜻으로, 처음 push할 때 한 번만 씁니다.

예:

```bash
git push -u origin study/kimjihoon-day1
```

> 처음 push할 때 로그인이나 토큰 입력이 필요할 수 있습니다. GitHub 비밀번호 대신 PAT를 사용할 수 있습니다.

### 4-9. GitHub에서 PR 만들기

1. GitHub에서 `travel-hunter-study` 저장소를 엽니다.
2. `Compare & pull request` 버튼이 보이면 클릭합니다.
3. 보이지 않으면 `Pull requests -> New pull request`로 들어갑니다.
4. PR 제목을 아래처럼 적습니다.

```text
docs: [git-session1] Git 기초와 첫 업로드
```

5. PR을 생성합니다.

## 5. 제출 문서 예시

아래는 완성된 제출 문서의 예시입니다. 실제 터미널 결과를 길게 붙여넣지 않아도 됩니다.

````md
# Git 기초 학습 Session 1 (YYYY-MM-DD)

## 오늘 배운 개념
- `git add`는 커밋할 파일을 올려두는 단계다.
- `git commit`은 변경 내용을 하나의 기록으로 남기는 단계다.
- `git push`는 내 컴퓨터의 커밋을 GitHub에 올리는 단계다.

## 오늘 실습할 명령어

```bash
git switch -c study/kimjihoon-day1
git status
git add members/kimjihoon/YYYY-MM-DD-git-session1.md
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"
git push -u origin study/kimjihoon-day1
```

## git status 확인

```text
새로 만든 제출 문서가 untracked files로 보였다.
```

## 직접 해본 것
- 새 브랜치를 만들었다.
- 제출 문서를 만들고 저장했다.
- `git status`로 새 파일 상태를 확인했다.
- `git add`로 파일을 스테이징했다.
- `git commit`으로 기록을 남겼다.
- `git push`로 GitHub에 올렸다.

## 헷갈렸던 점
- `git add`와 `git commit`은 역할이 다르다.

## 다음 학습 예정
- 다음 세션에서 `git pull`, 브랜치, PR 흐름을 더 익힐 예정이다.
````

## 6. 완료 체크리스트

아래 항목을 모두 만족하면 완료입니다.

- `members/<본인GitHub아이디>/YYYY-MM-DD-git-session1.md` 파일을 만들었다.
- `study/<아이디>-day1` 브랜치를 만들었다.
- 제출 문서를 먼저 완성하고 저장했다.
- `git status`를 실행했다.
- `git add`를 실행했다.
- `git commit`을 실행했다.
- `git push`를 실행했다.
- 실습 내용을 `.md` 문서에 정리했다.
- GitHub에 브랜치를 올렸다.
- PR을 생성했다.
