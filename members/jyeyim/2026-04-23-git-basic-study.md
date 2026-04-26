# Git 기초 학습 Session 1 (2026-04-23)

## 오늘 배운 개념
- `git add`는 커밋할 파일을 올려두는 것(repository로 올라간게 아님!)
- `git commit -m`은 변경 내용을 메모하여 알려주는 것
- `git push`는 내 컴퓨터의 커밋을 GitHub에 올리는 단계다. push까지 해줘야함.

## 오늘 실습한 명령어
git switch
git status
git add members/jyeyim/2026-04-24-git-session1.md
git commit -m "docs: [git-session1] Git 기초와 첫 업로드"
git push -u origin study/jyeyim-session1
```

## 실행 결과
text
Switched to a new branch 'study/jyeyim-session1'
Changes to be committed:
[study/jyeyim-session1] docs: [git-session1] Git 기초와 첫 업로드
branch 'study/jyeyim-session1' set up to track 'origin/study/jyeyim-session1'
```

## 직접 해본 것
- 새 브랜치를 만들었다.
- `git add`로 파일을 스테이징했다.
- `git commit`으로 기록을 남겼다.
- `git push`로 GitHub에 올렸다.

## 헷갈렸던 점
- git을 항상써줘야 되는점!

## 다음 학습 예정
- 다음 세션에서 `git pull`, 브랜치, PR 흐름을 더 익힐 예정이다.