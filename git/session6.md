# Session 6: 브랜치 전략과 협업 운영 정리

> 이번 세션의 목표는 지금까지 배운 Git/GitHub 흐름을 팀 프로젝트 운영 규칙으로 정리하고, 브랜치 전략과 merge 방식을 비교하는 것입니다.

## 이전 세션과 연결

Session 1-5에서 반복한 clone, branch, commit, push, PR, conflict, 복구 흐름을 이번 세션에서는 팀 규칙으로 묶어 봅니다. 어떤 브랜치 전략과 merge 방식을 선택할지 판단하는 것이 핵심입니다.

## 0. 시작 전

이 문서는 Git/GitHub 준비 과정의 마지막 정리 세션입니다.

- Session 1-5에서 사용한 `travel-hunter-study` 폴더를 VSCode에서 엽니다.
- 이번 세션은 새 명령어를 많이 외우는 것보다, 팀에서 어떤 규칙으로 협업할지 정리하는 데 목적이 있습니다.
- 실제 프로젝트 학습에 들어가기 전에 Git 작업 흐름을 스스로 설명할 수 있는지 확인합니다.

## 1. 제출 규격

| 항목 | 내용 |
|---|---|
| 파일 경로 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session6.md` |
| 브랜치 이름 | `study/<아이디>-day6` |
| PR 제목 | `docs: [git-session6] 브랜치 전략과 협업 운영 정리` |

## 2. 이번 세션에서 배우는 것

1. **GitHub Flow**: main과 짧은 작업 브랜치를 중심으로 운영하는 방식
2. **trunk-based 전략**: main에 자주 작게 합치는 방식
3. **Git Flow**: main, develop, feature, release, hotfix를 나누는 방식
4. **merge 방식**: merge commit, squash merge, rebase and merge의 차이
5. **tag**: 특정 시점의 코드를 버전 이름으로 기록하는 방법

## 3. 전체 흐름

```text
main 최신화 -> 새 브랜치 -> 협업 규칙 정리 -> merge 방식 비교 -> 최종 체크리스트 작성 -> add -> commit -> push -> PR
```

## 4. 실습 순서

### 4-1. `main` 최신화

```bash
git switch main
git pull origin main
```

### 4-2. 작업 브랜치 만들기

```bash
git switch -c study/<아이디>-day6
```

예:

```bash
git switch -c study/kimjihoon-day6
```

### 4-3. 브랜치 전략 비교

문서에 아래 표를 본인 말로 정리합니다.

| 전략 | 특징 | 적합한 상황 |
|---|---|---|
| GitHub Flow | main에서 브랜치를 만들고 PR로 합침 | 소규모 팀, 빠른 개발, 단순한 규칙 |
| trunk-based | main에 아주 자주 작게 합침 | 자동 테스트와 배포가 잘 갖춰진 팀 |
| Git Flow | main, develop, release, hotfix를 분리 | 릴리스 안정성이 중요한 제품 |

스터디와 작은 프로젝트에서는 보통 GitHub Flow가 가장 단순합니다.

전략을 고를 때는 아래 기준으로 판단합니다.

| 질문 | 추천 방향 |
|---|---|
| 팀 규모가 작고 배포 주기가 빠른가? | GitHub Flow |
| 자동 테스트와 배포가 잘 갖춰져 있고 아주 작은 단위로 합칠 수 있는가? | trunk-based |
| 릴리스, 핫픽스, 운영 버전을 엄격히 나눠야 하는가? | Git Flow |

스터디에서는 복잡한 브랜치 모델보다 `main` 최신화, 작은 PR, 리뷰 후 merge를 지키는 것이 더 중요합니다.

### 4-4. merge 방식 비교

GitHub PR에서 자주 보는 merge 방식은 다음과 같습니다.

| 방식 | 결과 | 특징 |
|---|---|---|
| Merge commit | 병합 커밋이 새로 생김 | 브랜치 작업 흔적이 남는다. |
| Squash merge | 여러 커밋을 하나로 합쳐 main에 반영 | main 기록이 깔끔해진다. |
| Rebase and merge | 커밋을 main 뒤에 이어 붙임 | 기록은 직선에 가깝지만 초보자에게는 어렵다. |

초보자는 팀 규칙이 없다면 기본 merge 또는 squash merge부터 이해하면 충분합니다.

merge 방식을 고를 때는 아래처럼 생각하면 됩니다.

| 상황 | 추천 방식 |
|---|---|
| 브랜치에서 어떤 커밋들이 있었는지 남기고 싶다 | Merge commit |
| 실습 커밋이 여러 개라 main 기록을 깔끔하게 남기고 싶다 | Squash merge |
| 팀이 Git 기록 정리에 익숙하고 직선형 히스토리를 원한다 | Rebase and merge |

초보자 스터디에서는 충돌 해결과 PR 리뷰 흐름을 먼저 익히고, rebase는 팀 규칙이 생긴 뒤 다루는 편이 안전합니다.

### 4-5. tag 개념 정리

tag는 특정 커밋에 버전 이름을 붙이는 기능입니다.  
프로젝트 단계가 끝났을 때 아래처럼 사용할 수 있습니다.

```bash
git tag -a v1.0 -m "1단계 완료"
git push origin v1.0
```

스터디에서는 꼭 실습하지 않아도 됩니다.  
다만 "중요한 완료 시점을 표시하는 이름표"라고 이해하면 됩니다.

### 4-6. 팀 협업 체크리스트 만들기

문서에 아래 항목을 본인 말로 정리합니다.

- 작업 시작 전 `main`에서 pull하기
- 새 브랜치에서 작업하기
- 커밋 메시지를 명확히 쓰기
- PR을 작게 만들기
- PR 올리기 전 스스로 diff 확인하기
- main에 직접 push하지 않기
- 민감 파일을 올리지 않기

### 4-7. 제출 문서 만들기

아래 경로로 파일을 만들고 내용을 채웁니다.

```text
members/<본인GitHub아이디>/YYYY-MM-DD-git-session6.md
```

### 4-8. add -> commit -> push -> PR

제출 문서를 저장한 뒤 아래 명령어를 실행합니다.
`git add` 이후 문서를 다시 수정했다면 `git add`를 한 번 더 실행합니다.

```bash
git status
git add members/<본인GitHub아이디>/YYYY-MM-DD-git-session6.md
git commit -m "docs: [git-session6] 브랜치 전략과 협업 운영 정리"
git push -u origin study/<아이디>-day6
```

push 후 GitHub에서 PR을 생성합니다.  
PR 제목: `docs: [git-session6] 브랜치 전략과 협업 운영 정리`

## 5. 제출 문서 예시

````md
# Git 기초 학습 Session 6 (YYYY-MM-DD)

## 오늘 배운 개념
- GitHub Flow는 main에서 브랜치를 만들고 PR로 합치는 단순한 전략이다.
- trunk-based는 main에 자주 작게 합치는 방식이다.
- Git Flow는 release, hotfix까지 나누는 더 복잡한 전략이다.
- tag는 특정 커밋에 버전 이름을 붙이는 기능이다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/kimjihoon-day6
git add members/kimjihoon/YYYY-MM-DD-git-session6.md
git commit -m "docs: [git-session6] 브랜치 전략과 협업 운영 정리"
git push -u origin study/kimjihoon-day6
```

## merge 방식 비교
- Merge commit: 브랜치가 합쳐진 기록이 남는다.
- Squash merge: 여러 커밋을 하나로 합쳐 main에 반영한다.
- Rebase and merge: 커밋을 main 뒤에 이어 붙인다.

## 선택 기준
- 작은 스터디와 빠른 협업에는 GitHub Flow가 가장 단순하다.
- main 기록을 깔끔하게 남기려면 squash merge를 고려할 수 있다.
- rebase는 팀 규칙이 정해진 뒤 사용하는 편이 안전하다.

## 내가 생각한 협업 규칙
- 작업 전에는 항상 main을 최신화한다.
- main에서 직접 작업하지 않는다.
- PR은 작게 만든다.
- push 전에 `git status`와 변경 내용을 확인한다.

## 최종 정리
- Git은 명령어를 외우는 것보다 현재 상태를 확인하면서 안전하게 작업하는 것이 중요하다.
````

## 6. 완료 체크리스트

- `study/<아이디>-day6` 브랜치를 만들었다.
- GitHub Flow, trunk-based, Git Flow의 차이를 정리했다.
- 브랜치 전략 선택 기준을 정리했다.
- merge 방식 3가지를 비교했다.
- merge 방식 선택 기준을 정리했다.
- tag가 무엇인지 문장으로 적었다.
- 팀 협업 체크리스트를 작성했다.
- `members/<본인GitHub아이디>/YYYY-MM-DD-git-session6.md` 파일을 만들었다.
- `git add`, `git commit`, `git push`를 실행했다.
- GitHub에 브랜치를 올리고 PR을 생성했다.
