# Session 3: 충돌과 merge 해결 실습

> 이번 세션의 목표는 Git 충돌(conflict)이 왜 생기는지 이해하고, 롤링페이퍼 파일에서 실제 충돌을 만든 뒤 직접 해결하는 흐름을 익히는 것입니다.

## 이전 세션과 연결

Session 2에서 브랜치와 PR 흐름을 익혔다면, 이번 세션에서는 여러 PR이 같은 파일의 같은 줄을 수정할 때 실제로 어떤 충돌이 생기는지 확인하고 해결합니다.

## 0. 시작 전

이 문서는 **Session 1, 2를 끝낸 뒤 이어서 진행하는 문서**입니다.

- Session 2에서 사용한 `travel-hunter-study` 폴더가 내 컴퓨터에 있어야 합니다.
- 이번 세션에서도 `main`을 최신화한 뒤 새 브랜치에서 작업합니다.
- 충돌은 반드시 전용 실습 파일인 `git/conflict-practice/rolling-paper.md`에서만 만듭니다.
- 충돌 해결의 목표는 한 사람의 내용만 남기는 것이 아니라, 가능한 모든 사람의 메시지를 보존하는 것입니다.

## 1. 제출 규격

| 항목 | 내용 |
|---|---|
| 파일 경로 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session3.md` |
| 브랜치 이름 | `study/<아이디>-day3` |
| PR 제목 | `docs: [git-session3] 충돌과 merge 해결 실습` |

## 2. 이번 세션에서 배우는 것

1. **merge**: 다른 브랜치의 변경 내용을 현재 브랜치로 합치는 작업
2. **conflict**: Git이 자동으로 합칠 수 없어 사람이 직접 선택해야 하는 상태
3. **conflict marker**: 충돌난 위치를 알려주는 `<<<<<<<`, `=======`, `>>>>>>>` 표시
4. **롤링페이퍼 충돌 실습**: 여러 사람이 같은 파일의 같은 줄을 수정해 실제 PR 충돌을 만든다
5. **충돌 해결 후 마무리**: 파일 수정 후 `git add`, `git commit`, `git push`

## 3. 전체 흐름

```text
main 최신화 -> 새 브랜치 생성 -> 롤링페이퍼 같은 줄 수정 -> 제출 문서 작성 -> commit/push/PR -> 첫 PR merge -> main merge로 충돌 해결 -> push -> PR 업데이트
```

이번 세션에서는 실제 프로젝트 코드가 아니라 전용 롤링페이퍼 파일만 수정합니다.  
첫 번째로 merge된 사람은 충돌이 나지 않을 수 있고, 두 번째 이후 사람은 같은 줄을 수정했기 때문에 충돌을 해결하게 됩니다.

## 4. 실습 순서

### 4-1. `travel-hunter-study` 폴더 열기

Session 2 때 사용한 폴더를 VSCode로 열고, `터미널 -> 새 터미널`을 누릅니다.

### 4-2. `main` 최신화

```bash
git switch main
git pull origin main
```

> 충돌을 줄이려면 새 작업을 시작하기 전에 항상 `main`을 최신화하는 습관이 필요합니다.

문서에는 아래를 적습니다.

- 실행한 명령어
- `Already up to date.` 또는 `Updating ...` 같은 결과 한 줄
- 왜 새 브랜치 전에 pull을 하는지 한 줄 설명

### 4-3. 작업 브랜치 만들기

```bash
git switch -c study/<아이디>-day3
```

예:

```bash
git switch -c study/kimjihoon-day3
```

`git switch`가 안 되면 아래 명령어를 사용해도 됩니다.

```bash
git checkout -b study/<아이디>-day3
```

### 4-4. 실제 충돌 만들기: 롤링페이퍼 실습

먼저 충돌이 났을 때 보게 될 표시를 읽어 둡니다.

```text
<<<<<<< HEAD
내 브랜치에서 작성한 내용
=======
합치려는 브랜치에서 작성한 내용
>>>>>>> main
```

의미는 다음과 같습니다.

- `<<<<<<< HEAD`: 현재 내 브랜치의 내용 시작
- `=======`: 두 버전의 경계
- `>>>>>>> main`: 합치려는 브랜치의 내용 끝

이제 실제 충돌을 만들기 위해 아래 파일을 엽니다.

```text
git/conflict-practice/rolling-paper.md
```

파일에는 처음에 아래 줄이 들어 있습니다.

```md
- 아직 작성되지 않음
```

모든 학습자는 이 **같은 줄**을 자기 메시지로 바꿉니다.

```md
- kimjihoon: Git 충돌 해결을 연습했습니다.
```

중요한 규칙:

- 새 줄을 추가하지 말고, 처음에는 반드시 `- 아직 작성되지 않음` 한 줄을 수정합니다.
- 운영자 또는 진행자는 첫 번째 PR 하나만 먼저 merge합니다.
- 두 번째 이후 PR은 같은 줄을 수정했기 때문에 GitHub에서 충돌이 표시됩니다.
- 최종 해결 결과는 한 사람만 남기는 것이 아니라 여러 사람의 메시지를 모두 남깁니다.

학습자가 많으면 2-4명 단위로 조를 나누고, 진행자가 조별 파일을 추가로 만들 수 있습니다.

```text
git/conflict-practice/rolling-paper-group-a.md
git/conflict-practice/rolling-paper-group-b.md
```

진행자는 아래 순서로 운영합니다.

1. 모든 학습자가 같은 `main` 기준에서 브랜치를 만들었는지 확인합니다.
2. 모든 학습자가 rolling paper의 같은 줄을 수정했는지 확인합니다.
3. 첫 번째 PR 하나만 먼저 merge합니다.
4. 나머지 PR에서 conflict 표시가 생기는지 확인합니다.
5. 충돌이 난 학습자는 로컬에서 `git merge main`으로 직접 해결합니다.

충돌이 안 난다면 아래를 확인합니다.

- 첫 PR이 실제로 `main`에 merge됐는지
- 내 브랜치가 첫 PR merge 이전의 `main`에서 만들어졌는지
- 모든 학습자가 같은 파일의 같은 줄을 수정했는지
- 새 줄을 추가한 것이 아니라 `- 아직 작성되지 않음` 줄을 바꿨는지

### 4-5. 제출 문서 만들기

VSCode에서 아래 경로로 파일을 만들고 내용을 채웁니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session3.md
```

문서에는 아래 내용을 포함합니다.

- `git pull`을 실행한 결과
- 만든 브랜치 이름
- rolling paper 파일에 적은 내 메시지
- conflict marker 3개의 의미
- 충돌 해결 순서
- merge와 conflict에 대해 이해한 점

### 4-6. 첫 commit, push, PR 만들기

```bash
git status
git add git/conflict-practice/rolling-paper.md
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session3.md
git commit -m "docs: [git-session3] 충돌과 merge 해결 실습"
git push -u origin study/<아이디>-day3
```

push 후 GitHub에서 PR을 생성합니다.  
PR 제목: `docs: [git-session3] 충돌과 merge 해결 실습`

첫 번째 PR은 진행자가 먼저 merge합니다.  
두 번째 이후 PR은 충돌이 표시될 수 있습니다.

### 4-7. `main`을 merge해서 충돌 해결하기

내 PR에 충돌이 표시되면 로컬에서 최신 `main`을 가져온 뒤, 내 브랜치에 merge합니다.

```bash
git switch main
git pull origin main
git switch study/<아이디>-day3
git merge main
```

충돌이 나면 `git/conflict-practice/rolling-paper.md` 파일에 conflict marker가 생깁니다.  
파일을 열고 두 사람의 메시지를 모두 남기도록 정리합니다.

예를 들어 아래처럼 바꿉니다.

```md
# Rolling Paper Conflict Practice

## 오늘의 한 줄
- 먼저 merge된 사람: Git 충돌 실습을 시작했습니다.
- kimjihoon: Git 충돌 해결을 연습했습니다.
```

충돌 표시인 `<<<<<<<`, `=======`, `>>>>>>>`는 최종 파일에 남기면 안 됩니다.

해결이 끝나면 아래 순서로 마무리합니다. 제출 문서도 수정했다면 `commit` 전에 함께 add합니다.

```bash
git add git/conflict-practice/rolling-paper.md
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session3.md
git commit
git push
```

`git commit`에서 편집기가 열리면 기본 merge commit 메시지를 저장하고 닫으면 됩니다.

충돌 해결 중 너무 꼬였다고 판단되면 merge를 취소하고 다시 시작할 수 있습니다.

```bash
git merge --abort
```

단, `git push -f`는 사용하지 않습니다.

## 5. 제출 문서 예시

````md
# Git 기초 학습 Session 3 (YYYY-MM-DD)

## 오늘 배운 개념
- merge는 다른 브랜치의 변경 내용을 현재 브랜치로 합치는 작업이다.
- conflict는 Git이 자동으로 합칠 수 없어 사람이 직접 정리해야 하는 상태다.
- 충돌 표시는 `<<<<<<<`, `=======`, `>>>>>>>` 형태로 나타난다.
- 충돌 해결은 둘 중 하나만 고르는 것이 아니라 필요한 내용을 모두 남기도록 정리하는 일이다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/kimjihoon-day3
git status
git add git/conflict-practice/rolling-paper.md
git add members/kimjihoon/YYYY-MM-DD-git-session3.md
git commit -m "docs: [git-session3] 충돌과 merge 해결 실습"
git push -u origin study/kimjihoon-day3
git switch main
git pull origin main
git switch study/kimjihoon-day3
git merge main
git add git/conflict-practice/rolling-paper.md
git commit
git push
```

## rolling paper에 적은 내 메시지
- `kimjihoon: Git 충돌 해결을 연습했습니다.`

## conflict marker 의미
- `<<<<<<< HEAD`: 현재 내 브랜치의 내용
- `=======`: 두 변경 내용의 경계
- `>>>>>>> main`: 합치려는 브랜치의 내용

## 충돌 해결 순서
1. 충돌난 파일을 연다.
2. 두 내용을 비교한다.
3. 남겨야 할 메시지를 모두 정리한다.
4. 충돌 표시를 삭제한다.
5. `git add`와 `git commit`으로 해결 내용을 기록한다.
6. `git push`로 PR을 업데이트한다.

## 헷갈렸던 점
- 충돌은 Git 오류가 아니라 사람이 최종 내용을 결정해야 하는 상황이라는 점을 알게 됐다.
````

## 6. 완료 체크리스트

- `git switch main`과 `git pull origin main`을 실행했다.
- `study/<아이디>-day3` 브랜치를 만들었다.
- `git/conflict-practice/rolling-paper.md`의 같은 줄을 내 메시지로 수정했다.
- `members/<본인GitHub아이디>/YYYY-MM-DD-git-session3.md` 파일을 만들었다.
- GitHub에 브랜치를 올리고 PR을 생성했다.
- 첫 PR이 merge된 뒤 내 브랜치에서 `git merge main`을 실행했다.
- conflict marker의 의미를 정리했다.
- 충돌 파일에서 모든 사람의 메시지를 보존하도록 정리했다.
- 충돌 표시를 최종 파일에서 삭제했다.
- `git add`, `git commit`, `git push`를 실행했다.
- `git push -f`를 사용하지 않았다.
