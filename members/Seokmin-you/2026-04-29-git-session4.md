# Git 기초 학습 Session 4 (2026-04-29)

## 오늘 배운 개념
- GitHub Flow는 main에서 브랜치를 만들고 PR로 다시 합치는 협업 방식이다.
- 브랜치 이름은 작업 내용을 알 수 있게 작성해야 한다.
- PR은 main에 합치기 전 리뷰를 받는 과정이다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/Seokmin-you-day4
git add members/Seokmin-you/2026-04-29-git-session4.md
git commit -m "docs: [git-session4] GitHub Flow와 PR 리뷰 워크플로우"
git push -u origin study/Seokmin-you-day4
```

## 좋은 브랜치 이름 예시
- `fix/conflict-error`
- `docs/commit-rule`

## 좋은 커밋 메시지 예시
- `docs: 커밋 규칙 정리`
- `fix: 컨플릭트 오류 수정`

## PR에 적을 내용
- 작업 내용
session 4 학습 내용 정리
브랜치 이름, 커밋 메시지, pr 작성의 좋은 예 와 나쁜 예 확인
- 확인 방법
Markdown 미리보기로 표와 코드 블록이 깨지지 않는지 확인했습니다.
`git status`로 제출 파일만 변경됐는지 확인했습니다.
- 리뷰어에게 확인받고 싶은 점
상단에 제시한 예시가 좋은 예시로 맞는지 확인 부탁드립니다.

## 리뷰할 때 주의할 점
- 사람을 지적하지 않고 코드나 문서에 대해 말한다.
- 고쳐야 하는 이유를 함께 적는다.

## 리뷰 반영 방법
- 같은 브랜치에서 수정한 뒤 추가 commit을 push하면 기존 PR이 업데이트된다.