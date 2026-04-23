# Git 기초 학습 Session 2 (2024-04-24)

## 오늘 배운 개념
- `git branch -d [브랜치명]는 git branch를 삭제하는 명령어이다.
- `git pull origin [브랜치명]`은 GitHub의 최신 내용을 내 컴퓨터로 가져오는 명령어이다.
- 브랜치는 main을 직접 건드리지 않고 따로 작업하는 공간이다.
- PR은 내 브랜치 작업을 main에 합쳐달라고 요청하는 단계다.

## 오늘 실습한 명령어

```bash
git switch main
git branch -d study/Halee0090-day1
git status
git pull origin main
git switch -c study/Halee0090-day2

```

## 실행 결과

```text
Switched to branch 'main'
warning: deleting branch 'study/Halee0090-day1' that has been merged to
'refs/remotes/origin/study/Halee0090-day1', but not yet merged to HEAD
Deleted branch study/Halee0090-day1 (was 9baf632).
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
Changes to be committed:
create mode 100644 members/Halee0090/2026-04-24-git-session1.md
Switched to a new branch 'study/Halee0090-day2'
```

## 직접 해본 것
- `git pull`로 main을 최신화했다.
- 새 브랜치를 만들어 그 안에서 작업했다.
- 로컬에서 커밋한 뒤 GitHub에 push했다.
- PR을 생성했다.

## 헷갈렸던 점
- `git pull`을 안 하고 브랜치를 따면 어떻게 되는지 궁금하다.

## 다음 학습 예정
- 다음 세션에서 충돌 해결과 merge 흐름을 익힐 예정이다.