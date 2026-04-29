# Git 기초 학습 Session 3 (YYYY-MM-DD)

## 오늘 배운 개념
- merge는 다른 브랜치의 변경 내용을 현재 브랜치로 합치는 작업이다.
- conflict는 Git이 자동으로 합칠 수 없어 사람이 직접 정리해야 하는 상태다.
- 충돌 표시는 `<<<<<<<`, `=======`, `>>>>>>>` 형태로 나타난다.
- 충돌 해결은 둘 중 하나만 고르는 것이 아니라 필요한 내용을 모두 남기도록 정리하는 일이다.

## 오늘 실습한 명령어

```bash
git remote -v
git clone https://github.com/travel-hunter/travel-hunter-study.git #온라인 연결이 풀린듯함 왜?
git switch main
git pull origin main
git switch -c study/Seokmin-you-day3
git status
git/conflict-practice/rolling-paper.md
git add members/Seokmin-you-git-session3.md
git commit -m "docs: [git-session3] 충돌과 merge 해결 실습"
git push -u origin study/Seokmin-you-day3
git switch main
git pull origin main
git switch study/Seokmin-you-day3
git merge main
git add git/conflict-practice/rolling-paper.md
git commit
git push
```

## rolling paper에 적은 내 메시지
- `coflict 충돌해결`

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
- 진행도중 왜 연결이 풀렸는지 그로인해 트러블 슈팅을 진행하였고 컨플릭트를 해결하는 실습을 하는데 혼자 하는것에 어려움이 있었음