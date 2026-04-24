# Session 4: GitHub Flow와 PR 리뷰 워크플로우

> 이번 세션의 목표는 팀 프로젝트에서 사용할 기본 협업 흐름을 이해하고, 좋은 브랜치 이름·커밋 메시지·PR 작성법을 익히는 것입니다.

## 이전 세션과 연결

Session 3에서 PR 충돌을 해결했다면, 이번 세션에서는 PR을 단순히 만드는 것에서 나아가 리뷰어가 이해하기 쉽게 작성하고, 리뷰를 받은 뒤 같은 브랜치에 추가 커밋으로 반영하는 흐름을 익힙니다.

## 0. 시작 전

이 문서는 `travel-hunter-study` 저장소에서 GitHub 협업 흐름을 연습하기 위한 문서입니다.

- Session 1-3에서 사용한 `travel-hunter-study` 폴더를 VSCode에서 엽니다.
- 이번 세션은 명령어보다 **협업 규칙을 이해하는 것**이 중요합니다.
- 실제 프로젝트에서는 main에 직접 push하지 않고, 브랜치와 PR을 통해 작업합니다.

## 1. 제출 규격

| 항목 | 내용 |
|---|---|
| 파일 경로 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session4.md` |
| 브랜치 이름 | `study/<아이디>-day4` |
| PR 제목 | `docs: [git-session4] GitHub Flow와 PR 리뷰 워크플로우` |

## 2. 이번 세션에서 배우는 것

1. **GitHub Flow**: main에서 새 브랜치를 만들고 PR로 합치는 단순한 협업 방식
2. **브랜치 네이밍**: 브랜치 이름만 봐도 작업 내용을 알 수 있게 쓰는 규칙
3. **커밋 메시지**: 변경 내용을 짧고 명확하게 남기는 규칙
4. **PR 리뷰**: main에 합치기 전 팀원이 확인하는 과정

## 3. 전체 흐름

```text
main 최신화 -> 브랜치 생성 -> 작업 -> commit -> push -> PR 생성 -> 리뷰 -> merge
```

스터디 저장소에서는 `study/<아이디>-day4` 형식을 사용합니다.  
실제 프로젝트에서는 보통 `feature/login`, `fix/token-error`, `docs/readme-update`처럼 작업 유형을 앞에 붙입니다.

## 4. 실습 순서

### 4-1. `main` 최신화

```bash
git switch main
git pull origin main
```

### 4-2. 작업 브랜치 만들기

```bash
git switch -c study/<아이디>-day4
```

예:

```bash
git switch -c study/kimjihoon-day4
```

### 4-3. 브랜치 이름 규칙 정리

실제 팀 프로젝트에서는 아래처럼 브랜치 이름을 정하면 좋습니다.

| 유형 | 용도 | 예시 |
|---|---|---|
| `feature/` | 새 기능 개발 | `feature/login` |
| `fix/` | 버그 수정 | `fix/token-error` |
| `docs/` | 문서 작업 | `docs/readme-update` |
| `refactor/` | 기능 변경 없는 코드 개선 | `refactor/api-structure` |
| `infra/` | 인프라 작업 | `infra/terraform-vpc` |
| `chore/` | 설정, 빌드, 패키지 관리 | `chore/update-deps` |

좋은 브랜치 이름은 "누가 봐도 어떤 작업인지 알 수 있는 이름"입니다.

### 4-4. 커밋 메시지 규칙 정리

커밋 메시지는 아래 형식을 권장합니다.

```text
타입: 변경 내용 요약
```

예:

```text
docs: GitHub Flow 정리
fix: 로그인 토큰 만료 처리 수정
feat: 사용자 프로필 API 추가
```

스터디 문서 제출에서는 아래 메시지를 사용합니다.

```bash
git commit -m "docs: [git-session4] GitHub Flow와 PR 리뷰 워크플로우"
```

### 4-5. PR 작성법 정리

PR은 "내 브랜치 작업을 main에 합쳐도 되는지 확인해 주세요"라는 요청입니다.

좋은 PR에는 아래 내용이 들어갑니다.

```md
## 작업 내용
- 무엇을 추가하거나 수정했는지

## 확인 방법
- 어떤 방식으로 확인했는지

## 궁금한 점
- 리뷰어에게 확인받고 싶은 부분
```

리뷰할 때는 사람을 평가하지 않고 코드와 문서에 대해 이야기합니다.

좋은 리뷰 예:

```text
이 문장은 초보자가 보기에는 어려울 수 있어서 예시를 하나 더 넣으면 어떨까요?
```

실무형 PR 본문은 아래처럼 조금 더 구체적으로 적습니다.

```md
## 작업 내용
- Session 4 학습 내용을 정리했습니다.
- 브랜치 이름, 커밋 메시지, PR 작성 기준을 예시와 함께 정리했습니다.

## 확인 방법
- Markdown 미리보기로 표와 코드 블록이 깨지지 않는지 확인했습니다.
- `git status`로 제출 파일만 변경됐는지 확인했습니다.

## 리뷰어에게 확인받고 싶은 점
- PR 설명이 처음 보는 사람도 이해할 수 있을 만큼 구체적인지 확인 부탁드립니다.
```

리뷰 코멘트를 받으면 같은 브랜치에서 파일을 수정하고 다시 commit/push합니다.  
새 PR을 만들 필요는 없고, 기존 PR이 자동으로 업데이트됩니다.

```bash
git status
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session4.md
git commit -m "docs: [git-session4] 리뷰 의견 반영"
git push
```

### 4-6. 제출 문서 만들기

아래 경로로 파일을 만들고 내용을 정리합니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session4.md
```

문서에는 아래 내용을 포함합니다.

- GitHub Flow 전체 흐름
- 좋은 브랜치 이름 예시 2개
- 좋은 커밋 메시지 예시 2개
- PR에 들어가야 할 내용
- 리뷰할 때 주의할 점
- 리뷰 코멘트를 받은 뒤 PR을 업데이트하는 방법

### 4-7. add -> commit -> push -> PR

```bash
git status
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session4.md
git commit -m "docs: [git-session4] GitHub Flow와 PR 리뷰 워크플로우"
git push -u origin study/<아이디>-day4
```

push 후 GitHub에서 PR을 생성합니다.  
PR 제목: `docs: [git-session4] GitHub Flow와 PR 리뷰 워크플로우`

## 5. 제출 문서 예시

````md
# Git 기초 학습 Session 4 (YYYY-MM-DD)

## 오늘 배운 개념
- GitHub Flow는 main에서 브랜치를 만들고 PR로 다시 합치는 협업 방식이다.
- 브랜치 이름은 작업 내용을 알 수 있게 작성해야 한다.
- PR은 main에 합치기 전 리뷰를 받는 과정이다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/kimjihoon-day4
git add members/kimjihoon/YYYY-MM-DD-git-session4.md
git commit -m "docs: [git-session4] GitHub Flow와 PR 리뷰 워크플로우"
git push -u origin study/kimjihoon-day4
```

## 좋은 브랜치 이름 예시
- `feature/login`
- `docs/readme-update`

## 좋은 커밋 메시지 예시
- `docs: PR 작성법 정리`
- `fix: 오타 수정`

## PR에 적을 내용
- 작업 내용
- 확인 방법
- 리뷰어에게 확인받고 싶은 점

## 리뷰할 때 주의할 점
- 사람을 지적하지 않고 코드나 문서에 대해 말한다.
- 고쳐야 하는 이유를 함께 적는다.

## 리뷰 반영 방법
- 같은 브랜치에서 수정한 뒤 추가 commit을 push하면 기존 PR이 업데이트된다.
````

## 6. 완료 체크리스트

- GitHub Flow 흐름을 문장으로 정리했다.
- 좋은 브랜치 이름 예시를 적었다.
- 좋은 커밋 메시지 예시를 적었다.
- PR 작성 항목을 정리했다.
- 리뷰할 때 주의할 점을 적었다.
- 리뷰 코멘트를 받은 뒤 같은 PR을 업데이트하는 흐름을 적었다.
- `members/<본인GitHub아이디>/YYYY-MM-DD-git-session4.md` 파일을 만들었다.
- `git add`, `git commit`, `git push`를 실행했다.
- GitHub에 브랜치를 올리고 PR을 생성했다.
