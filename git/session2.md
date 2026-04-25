# Session 2: 브랜치와 PR 실습

> 이번 세션의 목표는 `main`을 최신화하는 방법을 익히고, 브랜치와 PR이 왜 필요한지 이해하는 것입니다.

## 이전 세션과 연결

Session 1에서 제출 문서를 먼저 완성하고 `add -> commit -> push -> PR`로 올리는 흐름을 익혔다면, 이번 세션에서는 팀원이 merge한 최신 `main`을 가져온 뒤 새 브랜치에서 다시 작업하는 협업 루틴으로 확장합니다.

## 0. 시작 전

이 문서는 **Session 1을 끝낸 뒤 이어서 진행하는 문서**입니다.

- Session 1에서 사용한 `travel-hunter-study` 폴더가 내 컴퓨터에 있어야 합니다.
- 아직 없다면 [session1.md](./session1.md)를 먼저 진행합니다.

## 1. 제출 규격

| 항목 | 내용 |
|---|---|
| 파일 경로 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md` |
| 브랜치 이름 | `study/<아이디>-day2` |
| PR 제목 | `docs: [git-session2] 브랜치와 PR 실습` |

## 2. 이번 세션에서 새로 배우는 것

Session 1에서는 제출 문서를 먼저 완성한 뒤 `add -> commit -> push -> PR`로 올리는 흐름을 익혔습니다.
이번 세션에서 추가로 배우는 것은 아래 두 가지입니다.

1. **`git pull`**: 다른 사람이 올린 최신 변경을 내 컴퓨터로 가져오는 방법
2. **브랜치 전략**: 왜 `main`에서 직접 작업하지 않고 새 브랜치를 따는지

## 3. 전체 흐름

```
main 최신화 → 새 브랜치 → 제출 문서 작성/저장 → add → commit → push → PR
```

Session 1과 달라진 점은 맨 앞에 **`main` 최신화** 단계가 추가된 것입니다.

## 4. 실습 순서

### 4-1. `travel-hunter-study` 폴더 열기

Session 1 때 사용한 폴더를 VSCode로 열고, `터미널 -> 새 터미널`을 누릅니다.

### 4-2. `main` 최신화 ← 이번 세션 핵심

터미널에서 아래 명령어를 순서대로 실행합니다.

```bash
git switch main
git pull origin main
```

**왜 이 단계가 필요한가?**

팀원들이 Session 1 PR을 merge했다면, 내 로컬 `main`은 GitHub보다 뒤처진 상태입니다.  
`git pull`은 GitHub의 최신 내용을 내 컴퓨터로 가져와 이 간격을 좁힙니다.

```
GitHub main:  A - B - C  (팀원 커밋이 쌓여 있음)
내 로컬 main: A           (아직 옛날 상태)
                          ← git pull이 B, C를 가져옴
내 로컬 main: A - B - C  (최신화 완료)
```

> `git pull origin main`은 `git fetch` + `git merge`를 한 번에 실행하는 단축 명령어입니다.

결과 확인:
- `Already up to date.` → 이미 최신 상태
- `Updating abc1234..def5678` → 새 커밋이 반영됨

나중에 제출 문서에 아래 내용을 정리합니다.
- 실행한 명령어
- 나온 결과 한 줄
- `git pull`이 무엇을 하는지 한 줄 설명

### 4-3. 새 브랜치 만들기 ← 이번 세션 핵심

```bash
git switch -c study/<아이디>-day2
```

**왜 `main`에서 직접 작업하지 않는가?**

`main`은 팀 전체가 공유하는 기준 브랜치입니다.  
여기에 바로 커밋하면 내 실수가 즉시 팀 전체에 영향을 줍니다.

브랜치는 `main`의 복사본 위에서 독립적으로 작업하는 공간입니다.

```
main:         A - B - C
                        \
내 브랜치:               D - E  (내 작업은 여기에만 쌓임)
```

작업이 끝나면 PR을 통해 코드 리뷰를 거친 뒤 `main`에 합칩니다.  
PR은 "내 브랜치 작업을 main에 합쳐달라"는 요청입니다.

`git switch`가 안 되면:

```bash
git checkout -b study/<아이디>-day2
```

나중에 제출 문서에 아래 내용을 정리합니다.
- 만든 브랜치 이름
- 왜 `main`이 아니라 새 브랜치에서 작업하는지 한 줄 설명

### 4-4. 제출 문서 만들기

VSCode에서 아래 경로로 파일을 만들고, Section 5의 예시를 참고해 내용을 채웁니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md
```

### 4-5. add → commit → push → PR

Session 1처럼 제출 문서를 먼저 완성하고 저장한 뒤 GitHub에 올립니다.
`git add` 이후 문서를 다시 수정했다면 `git add`를 한 번 더 실행해야 합니다.

```bash
git status
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md
git commit -m "docs: [git-session2] 브랜치와 PR 실습"
git push -u origin study/<아이디>-day2
```

push 후 GitHub에서 `Compare & pull request` 버튼을 눌러 PR을 생성합니다.  
PR 제목: `docs: [git-session2] 브랜치와 PR 실습`

## 5. 제출 문서 예시

````md
# Git 기초 학습 Session 2 (YYYY-MM-DD)

## 오늘 배운 개념
- `git pull`은 GitHub의 최신 내용을 내 컴퓨터로 가져오는 명령어다.
- 브랜치는 main을 직접 건드리지 않고 따로 작업하는 공간이다.
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

## main 최신화와 브랜치 확인

```text
Already up to date.
Switched to a new branch 'study/kimjihoon-day2'
```

## 직접 해본 것
- `git pull`로 main을 최신화했다.
- 새 브랜치를 만들어 그 안에서 작업했다.
- 제출 문서를 완성하고 저장했다.
- 로컬에서 커밋한 뒤 GitHub에 push했다.
- PR을 생성했다.

## 헷갈렸던 점
- `git pull`을 안 하고 브랜치를 따면 어떻게 되는지 궁금하다.

## 다음 학습 예정
- 다음 세션에서 충돌 해결과 merge 흐름을 익힐 예정이다.
````

## 6. 완료 체크리스트

- `git switch main`과 `git pull origin main`을 실행했다.
- `study/<아이디>-day2` 브랜치를 만들었다.
- 브랜치가 왜 필요한지 문장으로 적었다.
- PR이 어떤 역할을 하는지 문장으로 적었다.
- `members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md` 파일을 만들었다.
- 제출 문서를 완성하고 저장했다.
- `git add`, `git commit`, `git push`를 실행했다.
- GitHub에 브랜치를 올리고 PR을 생성했다.
