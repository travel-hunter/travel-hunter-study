# Git 기초 학습 Session 5 (2026-04-29)

## 오늘 배운 개념
- `git restore`는 파일 변경이나 staging 상태를 되돌릴 때 사용한다.
- `git revert`는 기존 커밋을 취소하는 새 커밋을 만든다.
- `git reset --hard`는 작업 내용을 지울 수 있어서 조심해야 한다.
- `git stash`는 커밋 전 작업을 잠시 보관한다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/Seokmin-you-day5
git status
git stash -u
git stash list
git stash pop
git add members/Seokmin-you/2026-04-29-git-session5.md
git commit -m "docs: [git-session5] 실수 복구와 stash 실습"
git push -u origin study/Seokmin-you-day5
```

## 복구 명령어 정리
- `git restore` # 파일 변경을 되돌리거나 staging 을 취소할때 사용 작업 내용이 사라질수 있으니 diff 확인 후 사용한다.
- `git revert` # 이미 만든 커밋을 취소하는 새 커밋을 만들 때 사용 이미 push 한 커밋을 되돌릴 때 안전하다.
- `git reset` # 커밋 위치를 과거로 옮길 때 사용 --hard 는 작업파일 까지 지울수 있다.
- `git stash` # 커밋 전 작업을 잠시 보관할 때 사용 pop 할때 충돌이 날 수 있다.

## 조심해야 할 명령어
- `git reset --hard`
- `git clean -fd`
- `git push -f`

## 상황별 선택 기준
- 이미 push한 커밋은 `revert`로 되돌린다.
- 아직 commit하지 않은 파일 변경은 `restore`로 되돌린다.
- commit하기 애매한 작업은 `stash`로 잠시 보관한다.

## 헷갈렸던 점
- 비슷한 기능이 여러개 있는것은 나눠서 사용해야 할 이유가 있기 때문임으로 외우고 상황에 맞게 활용함이 필요해 보인다.