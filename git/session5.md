# Session 5: 실수 복구와 stash 실습

> 이번 세션의 목표는 Git에서 실수했을 때 사용할 수 있는 복구 명령어의 차이를 이해하고, 작업 중인 내용을 stash로 잠시 보관하는 방법을 익히는 것입니다.

## 이전 세션과 연결

Session 4까지는 PR을 잘 만들고 리뷰받는 흐름을 익혔습니다. 이번 세션에서는 그 과정에서 파일을 잘못 수정했거나, 커밋을 되돌려야 하거나, 작업을 잠시 치워야 할 때 어떤 명령어를 선택할지 정리합니다.

## 0. 시작 전

이 문서는 `travel-hunter-study` 저장소에서 안전하게 복구 개념을 익히기 위한 문서입니다.

- 이번 세션에서는 `reset --hard`, `git clean`, 강제 push를 실행하지 않습니다.
- 위험한 명령어는 의미만 이해하고, 실제 프로젝트에서는 신중하게 사용합니다.
- 직접 실행하는 핵심 실습은 `git stash -u`, `git stash list`, `git stash pop`입니다.

## 1. 제출 규격

| 항목 | 내용 |
|---|---|
| 파일 경로 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session5.md` |
| 브랜치 이름 | `study/<아이디>-day5` |
| PR 제목 | `docs: [git-session5] 실수 복구와 stash 실습` |

## 2. 이번 세션에서 배우는 것

1. **restore**: 파일 변경 또는 staging 상태를 되돌릴 때 사용
2. **revert**: 이미 만든 커밋을 취소하는 새 커밋을 만들 때 사용
3. **reset**: 브랜치 위치와 작업 상태를 과거로 옮길 때 사용
4. **stash**: 아직 커밋하지 않은 작업을 잠시 보관할 때 사용

## 3. 전체 흐름

```text
main 최신화 -> 새 브랜치 -> 제출 문서 일부 작성 -> stash로 보관 -> 다시 꺼내기 -> 문서 완성 -> add -> commit -> push -> PR
```

## 4. 실습 순서

### 4-1. `main` 최신화

```bash
git switch main
git pull origin main
```

### 4-2. 작업 브랜치 만들기

```bash
git switch -c study/<아이디>-day5
```

예:

```bash
git switch -c study/kimjihoon-day5
```

### 4-3. 제출 문서 일부 작성

아래 경로로 파일을 만들고, 제목과 `오늘 배운 개념` 일부만 먼저 작성합니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session5.md
```

그다음 상태를 확인합니다.

```bash
git status
```

### 4-4. stash로 작업 잠시 보관하기

새로 만든 파일은 아직 Git이 추적하지 않는 untracked 파일입니다.  
그래서 `-u` 옵션을 붙여 stash에 함께 보관합니다.

```bash
git stash -u
git status
git stash list
```

확인할 것:

- 작업 파일이 잠시 사라졌는지
- `git stash list`에 stash 기록이 보이는지

### 4-5. stash 다시 꺼내기

```bash
git stash pop
git status
```

확인할 것:

- 보관했던 문서가 다시 나타났는지
- `git status`에서 변경 파일이 보이는지

### 4-6. 복구 명령어 차이 정리

문서에 아래 내용을 본인 말로 정리합니다.

| 명령어 | 언제 쓰나 | 주의점 |
|---|---|---|
| `git restore` | 파일 변경을 되돌리거나 staging을 취소할 때 | 작업 내용이 사라질 수 있으니 diff 확인 후 사용 |
| `git revert` | 이미 만든 커밋을 취소하는 새 커밋을 만들 때 | 이미 push한 커밋을 되돌릴 때 안전함 |
| `git reset` | 커밋 위치를 과거로 옮길 때 | `--hard`는 작업 파일까지 지울 수 있음 |
| `git stash` | 커밋 전 작업을 잠시 보관할 때 | pop할 때 충돌이 날 수 있음 |

상황별 선택 기준은 아래처럼 정리하면 됩니다.

| 상황 | 먼저 고려할 명령어 | 이유 |
|---|---|---|
| 아직 commit하지 않은 파일 변경을 취소하고 싶다 | `git restore <파일>` | working tree의 변경만 되돌릴 수 있음 |
| `git add`한 파일을 staging에서 내리고 싶다 | `git restore --staged <파일>` | 파일 내용은 유지하고 staging만 취소함 |
| 이미 push한 커밋을 취소하고 싶다 | `git revert <커밋해시>` | 공유된 기록을 지우지 않고 취소 커밋을 남김 |
| 아직 push하지 않은 로컬 커밋을 다시 정리하고 싶다 | `git reset --soft` 또는 `git reset --mixed` | 로컬 기록만 정리할 때 사용 가능 |
| 하던 작업을 잠시 치우고 브랜치를 바꾸고 싶다 | `git stash -u` | commit하기 애매한 변경을 임시 보관함 |

초보자는 실제 협업 브랜치에서 `reset --hard`와 강제 push를 복구 수단으로 쓰지 않는다고 기억하면 됩니다.

### 4-7. 위험한 명령어 주의

아래 명령어는 실제 협업 저장소에서 바로 실행하지 않습니다.

```bash
git reset --hard
git clean -fd
git push -f
```

특히 이미 GitHub에 올린 커밋은 지우기보다 아래처럼 새 커밋으로 되돌리는 편이 안전합니다.

```bash
git revert <커밋해시>
```

### 4-8. add -> commit -> push -> PR

stash에서 꺼낸 제출 문서를 완성하고 저장한 뒤 아래 명령어를 실행합니다.
`git add` 이후 문서를 다시 수정했다면 `git add`를 한 번 더 실행합니다.

```bash
git status
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session5.md
git commit -m "docs: [git-session5] 실수 복구와 stash 실습"
git push -u origin study/<아이디>-day5
```

push 후 GitHub에서 PR을 생성합니다.  
PR 제목: `docs: [git-session5] 실수 복구와 stash 실습`

## 5. 제출 문서 예시

````md
# Git 기초 학습 Session 5 (YYYY-MM-DD)

## 오늘 배운 개념
- `git restore`는 파일 변경이나 staging 상태를 되돌릴 때 사용한다.
- `git revert`는 기존 커밋을 취소하는 새 커밋을 만든다.
- `git reset --hard`는 작업 내용을 지울 수 있어서 조심해야 한다.
- `git stash`는 커밋 전 작업을 잠시 보관한다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/kimjihoon-day5
git status
git stash -u
git stash list
git stash pop
git add members/kimjihoon/YYYY-MM-DD-git-session5.md
git commit -m "docs: [git-session5] 실수 복구와 stash 실습"
git push -u origin study/kimjihoon-day5
```

## 복구 명령어 정리
- `restore`: 커밋 전 파일 변경을 되돌릴 때 사용한다.
- `revert`: 이미 공유된 커밋을 안전하게 취소할 때 사용한다.
- `reset`: 커밋 위치를 옮기는 명령이라 조심해서 써야 한다.
- `stash`: 아직 커밋하지 않은 작업을 잠시 보관할 때 사용한다.

## 조심해야 할 명령어
- `git reset --hard`
- `git clean -fd`
- `git push -f`

## 상황별 선택 기준
- 이미 push한 커밋은 `revert`로 되돌린다.
- 아직 commit하지 않은 파일 변경은 `restore`로 되돌린다.
- commit하기 애매한 작업은 `stash`로 잠시 보관한다.

## 헷갈렸던 점
- 되돌리기 명령어들이 비슷해 보여도 실제로 바꾸는 범위가 다르다는 점이 중요하다.
````

## 6. 완료 체크리스트

- `study/<아이디>-day5` 브랜치를 만들었다.
- 제출 문서를 일부 작성한 뒤 `git stash -u`를 실행했다.
- `git stash list`로 보관된 작업을 확인했다.
- `git stash pop`으로 작업을 다시 꺼냈다.
- `restore`, `revert`, `reset`, `stash`의 차이를 정리했다.
- 상황별로 어떤 복구 명령어를 먼저 쓸지 정리했다.
- 위험한 명령어 3개를 적었다.
- `members/<본인GitHub아이디>/YYYY-MM-DD-git-session5.md` 파일을 완성했다.
- `git add`, `git commit`, `git push`를 실행했다.
- GitHub에 브랜치를 올리고 PR을 생성했다.
