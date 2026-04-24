# Session 7: Git 기본기 보강 - init, ignore, diff, log

> 이번 세션의 목표는 새 Git 저장소를 직접 만들어 보고, `.gitignore`, `diff`, `log`를 통해 커밋 전후 상태를 확인하는 습관을 익히는 것입니다.

## 이전 세션과 연결

Session 6에서 협업 체크리스트에 넣은 "PR 올리기 전 스스로 diff 확인하기"를 실제 명령어로 연습합니다. 또한 새 프로젝트를 시작할 때 필요한 `git init`과 `.gitignore` 기본기도 함께 보강합니다.

## 0. 시작 전

이 문서는 **Session 1-6까지 진행한 뒤 부족한 기본기를 보강하는 문서**입니다.

- 기존에 clone한 `travel-hunter-study` 폴더 안에서는 `git init`을 실행하지 않습니다.
- `git init` 실습은 `travel-hunter-study` 폴더 바깥에 별도 연습 폴더를 만들어 진행합니다.
- 실제 제출물은 기존처럼 `travel-hunter-study` 저장소의 `members/` 폴더에 작성합니다.

## 1. 제출 규격

| 항목 | 내용 |
|---|---|
| 파일 경로 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session7.md` |
| 브랜치 이름 | `study/<아이디>-day7` |
| PR 제목 | `docs: [git-session7] Git 기본기 보강 - init, ignore, diff, log` |

## 2. 이번 세션에서 배우는 것

1. **`git init`**: 일반 폴더를 Git 저장소로 만드는 명령어
2. **`.gitignore`**: Git에 올리지 않을 파일과 폴더를 정하는 파일
3. **`git diff`**: 아직 staging하지 않은 변경 내용을 확인하는 명령어
4. **`git diff --staged`**: staging area에 올라간 변경 내용을 확인하는 명령어
5. **`git log --oneline --graph --all`**: 커밋 기록과 브랜치 흐름을 한눈에 확인하는 명령어

## 3. 전체 흐름

```text
main 최신화 -> 작업 브랜치 생성 -> 별도 연습 폴더 생성 -> git init 실습 -> .gitignore 작성 -> diff/log 확인 -> 제출 문서 작성 -> add -> commit -> push -> PR
```

이번 세션의 핵심은 "커밋하기 전에 무엇이 바뀌었는지 확인한다"입니다.

## 4. 실습 순서

### 4-1. `main` 최신화

먼저 `travel-hunter-study` 폴더에서 시작합니다.

```bash
git switch main
git pull origin main
```

### 4-2. 작업 브랜치 만들기

```bash
git switch -c study/<아이디>-day7
```

예시:

```bash
git switch -c study/kimjihoon-day7
```

### 4-3. 별도 연습 폴더에서 `git init` 실행하기

`git init`은 이미 Git 저장소인 `travel-hunter-study` 안에서 실행하지 않습니다.  
아래처럼 `travel-hunter-study`의 상위 폴더로 나가서 별도 연습 폴더를 만듭니다.

```bash
cd ..
mkdir git-init-practice-session7
cd git-init-practice-session7
git init
git status
```

`git init`을 실행하면 해당 폴더 안에 `.git` 폴더가 생기고, Git이 이 폴더의 변경사항을 추적할 수 있게 됩니다.

제출 문서에는 아래 내용을 적습니다.

- `git init`을 실행한 폴더 이름
- `git status` 결과
- `git init`이 무엇을 하는지 한 줄 설명

### 4-4. `.gitignore` 작성하기

연습 폴더 안에서 아래 파일과 폴더를 만듭니다.

```text
memo.md
.env
logs/app.log
.gitignore
```

`.gitignore` 파일에는 아래 내용을 적습니다.

```gitignore
.env
logs/
```

그다음 상태를 확인합니다.

```bash
git status
git status --ignored
```

확인할 점:

- `.env`는 비밀번호, API 키 같은 민감한 값을 담을 수 있어서 Git에 올리지 않습니다.
- `logs/`는 실행 중 생기는 기록 파일이라 Git에 올리지 않습니다.
- `.gitignore` 자체는 저장소 규칙이므로 커밋 대상입니다.

### 4-5. `git diff --staged`로 커밋 전 확인하기

`memo.md`에 아래 내용을 적습니다.

```md
# Git init practice

- git init은 일반 폴더를 Git 저장소로 만든다.
- .gitignore는 Git에 올리지 않을 파일을 정한다.
```

이제 커밋할 파일만 staging합니다.

```bash
git add memo.md .gitignore
git diff --staged
```

`git diff --staged`는 "이번 커밋에 들어갈 변경 내용"을 보여줍니다.  
커밋하기 전에 이 명령어로 실수로 들어간 내용이 없는지 확인합니다.

이상이 없으면 커밋합니다.

```bash
git commit -m "docs: init and ignore practice"
```

### 4-6. `git diff`로 아직 add하지 않은 변경 확인하기

방금 커밋한 뒤 `memo.md`에 아래 한 줄을 추가합니다.

```md
- git diff는 아직 add하지 않은 변경 내용을 보여준다.
```

아직 `git add`를 하지 않은 상태에서 diff를 확인합니다.

```bash
git diff
```

확인할 점:

- `git diff`는 working tree의 변경을 보여줍니다.
- `git diff --staged`는 staging area에 올라간 변경을 보여줍니다.
- 둘은 확인하는 위치가 다릅니다.

변경 내용을 확인했다면 두 번째 커밋을 만듭니다.

```bash
git add memo.md
git diff --staged
git commit -m "docs: diff practice note"
```

### 4-7. `git log --oneline --graph --all`로 기록 확인하기

```bash
git log --oneline --graph --all
```

이 명령어는 커밋 기록을 짧고 그래프 형태로 보여줍니다.

제출 문서에는 아래 내용을 적습니다.

- 출력된 커밋 개수
- 가장 최근 커밋 메시지
- `--oneline`, `--graph`, `--all` 옵션의 의미

### 4-8. `travel-hunter-study`로 돌아와 제출 문서 만들기

연습 폴더에서 실습한 내용은 별도 Git 저장소에 남겨둡니다.  
제출 문서는 다시 `travel-hunter-study` 저장소에 작성합니다.

```bash
cd ../travel-hunter-study
```

아래 경로에 제출 문서를 만듭니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session7.md
```

제출 문서에는 아래 내용을 포함합니다.

- `git init` 실습 결과
- `.gitignore`에 적은 내용
- `git diff`와 `git diff --staged`의 차이
- `git log --oneline --graph --all` 실행 결과
- 이번 세션에서 가장 중요하다고 느낀 점

### 4-9. add -> commit -> push -> PR

여기서부터는 별도 연습 폴더가 아니라 `travel-hunter-study` 저장소에서 실행합니다.
제출 문서를 저장한 뒤 진행하고, `git add` 이후 문서를 다시 수정했다면 `git add`를 한 번 더 실행합니다.

```bash
git status
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session7.md
git commit -m "docs: [git-session7] Git 기본기 보강 - init, ignore, diff, log"
git push -u origin study/<아이디>-day7
```

push 후 GitHub에서 PR을 생성합니다.  
PR 제목: `docs: [git-session7] Git 기본기 보강 - init, ignore, diff, log`

## 5. 제출 문서 예시

````md
# Git 기초 학습 Session 7 (YYYY-MM-DD)

## 오늘 배운 개념
- `git init`은 일반 폴더를 Git 저장소로 만드는 명령어다.
- `.gitignore`는 Git에 올리지 않을 파일과 폴더를 정한다.
- `git diff`는 아직 add하지 않은 변경을 확인한다.
- `git diff --staged`는 커밋에 들어갈 변경을 확인한다.
- `git log --oneline --graph --all`은 커밋 흐름을 짧게 보여준다.

## 오늘 실습한 명령어

```bash
git init
git status
git status --ignored
git add memo.md .gitignore
git diff --staged
git commit -m "docs: init and ignore practice"
git diff
git add memo.md
git commit -m "docs: diff practice note"
git log --oneline --graph --all
```

## .gitignore에 적은 내용

```gitignore
.env
logs/
```

## diff 명령어 차이
- `git diff`: 아직 staging하지 않은 변경을 확인한다.
- `git diff --staged`: staging area에 올라간 변경을 확인한다.

## log 실행 결과

```text
* abc1234 docs: diff practice note
* def5678 docs: init and ignore practice
```

## 내가 이해한 것
- 커밋하기 전에 `git diff --staged`로 실제 커밋 내용을 확인해야 한다.
- `.env` 같은 민감한 파일은 `.gitignore`로 제외해야 한다.

## 헷갈렸던 점
- `git diff`와 `git diff --staged`가 보는 위치가 다르다는 점이 헷갈렸다.
````

## 6. 완료 체크리스트

- `travel-hunter-study` 밖에 별도 연습 폴더를 만들었다.
- 별도 연습 폴더에서 `git init`을 실행했다.
- `.gitignore`에 `.env`, `logs/`를 추가했다.
- `git status --ignored`로 무시되는 파일을 확인했다.
- `git diff`와 `git diff --staged`를 각각 실행했다.
- `git log --oneline --graph --all`로 커밋 기록을 확인했다.
- `travel-hunter-study`로 돌아와 제출 문서를 만들었다.
- `study/<아이디>-day7` 브랜치에서 작업했다.
- `git add`, `git commit`, `git push`를 실행했다.
- GitHub에 브랜치를 올리고 PR을 생성했다.
