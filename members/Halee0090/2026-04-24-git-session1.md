# Git 기초 학습 Session 1 (2024-04-24)

## 오늘 배운 개념
- git을 사용하는 이유는 파일 변경내역 보존하고 관리하는 과정 필요하기 때문이다.
- 작업 폴더에서 git 쓰고 싶으면 `git init`, 원격저장소를 로컬 저장소로 사용하고 싶으면 `git clone [복사한 URL] [DIR]`
- `git add [파일명]`는 로컬 저장소에 있는 파일을 스테이징하는 것이다.
- `git commit`은 스테이징한 파일들을 repository에 저장하는 것이다.
- `git log --all -oneline`은 commit 내역을 조회하는 것이다.


## 오늘 실습한 명령어

```bash
git clone https://github.com/travel-hunter/travel-hunter-study.git
git branch study/Halee0090-day1
git switch study/Halee0090-day1
git status
git add 2026-04-24-git-session1.md
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"
git push -u origin study/Halee0090-day1

```

## 실행 결과

```text
Switched to a new branch 'study/kimjihoon-day1'
Changes to be committed:
[study/kimjihoon-day1 abc1234] docs: [git-session1] Git 기초와 첫 업로드
branch 'study/kimjihoon-day1' set up to track 'origin/study/kimjihoon-day1'
```

## 직접 해본 것
- 새 브랜치를 만들었다.
- `git add`로 파일을 스테이징했다.
- `git commit`으로 기록을 남겼다.
- `git push`로 GitHub에 올렸다.
 - github에서 pull request, merge 작업을 하여 main branch에 업데이트

## 헷갈렸던 점
- `git add`와 `git commit`은 역할이 다르다.

## 다음 학습 예정
- 다음 세션에서 `git pull`, 브랜치, PR 흐름을 더 익힐 예정이다.