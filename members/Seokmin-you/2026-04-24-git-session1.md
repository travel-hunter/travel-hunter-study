# Git 기초 학습 Session 1 (2026-04-24)

## 오늘 배운 개념
- `git add`는 커밋할 파일을 올려두는 단계다.
- `git commit`은 변경 내용을 하나의 기록으로 남기는 단계다.
- `git push`는 내 컴퓨터의 커밋을 GitHub에 올리는 단계다.

## 오늘 실습한 명령어

```bash
git switch -c study/Seokmin-you-day1
git status
git add members/Seokmin-you/2026-04-24-git-session1.md
git status
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"
git push -u origin study/Seokmin-you-day1
```

## 실행 결과

```text
Switched to a new branch 'study/Seokmin-you-day1'
On branch study/Seokmin-you-day1
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   2026-04-24-git-session1.md

no changes added to commit (use "git add" and/or [study/Seokmin-you-day1 4ac1cbb] docs: [git-session1] Git 기초와 첫 업로드
 1 file changed, 50 deletions(-)
 "git commit -a")
   45d6dec..4ac1cbb  study/Seokmin-you-day1 -> study/Seokmin-you-day1
branch 'study/Seokmin-you-day1' set up to track 'origin/study/Seokmin-you-day1'.
```

## 직접 해본 것
- 새 브랜치를 만들었다.
- `git add`로 파일을 스테이징했다.
- `git status`로 스테이징 된 파일의 상태를 확인했다.
- `git commit`으로 기록을 남겼다.
- `git push`로 GitHub에 올렸다.

## 헷갈렸던 점
- `git add`실행시 파일을 만드는것이 아니라 스테이징에 추가하는것.

## 다음 학습 예정
- 다음 세션에서 `git pull`, 브랜치, PR 흐름을 더 익힐 예정이다.