# Git 기초 학습 Session 1 (YYYY-MM-DD)

## 오늘 배운 개념
- `git add`는 커밋할 파일을 올려두는 단계다.
- `git commit`은 변경 내용을 하나의 기록으로 남기는 단계다.
- `git push`는 내 컴퓨터의 커밋을 GitHub에 올리는 단계다.

## 오늘 실습한 명령어

```bash
git switch -c study/kimjihoon-day1
git status
git add members/kimjihoon/YYYY-MM-DD-git-session1.md
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"
git push -u origin study/kimjihoon-day1
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

## 헷갈렸던 점
- `git add`와 `git commit`은 역할이 다르다.

## 다음 학습 예정
- 다음 세션에서 `git pull`, 브랜치, PR 흐름을 더 익힐 예정이다.