# Git 기초 학습 Session 6 (2026-05-06)

## 오늘 배운 개념
- GitHub Flow는 main에서 브랜치를 만들고 PR로 합치는 단순한 전략이다.
- trunk-based는 main에 자주 작게 합치는 방식이다.
- Git Flow는 release, hotfix까지 나누는 더 복잡한 전략이다.
- tag는 특정 커밋에 버전 이름을 붙이는 기능이다.

## 오늘 실습한 명령어

```bash
git switch main
git pull origin main
git switch -c study/Seokmin-you-day6
git add members/Seokmin-you/2026-05-06-git-session6.md
git commit -m "docs: [git-session6] 브랜치 전략과 협업 운영 정리"
git push -u origin study/Seokmin-you-day6
```
## 브랜치 전략 비교
- GitHub Flow : main에서 브랜치를 만들고 PR로 합친다. 소규모 팀, 빠른개발, 단순한 규칙에 적합하다.
- trunk-based : main에 아주 작게 합친다. 자동 테스트와 배포가 잘 갖춰진 팀에서 쓰인다.
- Git Flow : main, develop, release, hotfix를 분리한다. 릴리스 안정성이 중요한 제품이다.

## 전략을 고르는 기준
- 팀 규모가 작고 배포 주기가 빠른가?  > GitHub Flow
- 자동 테스트와 배포가 잘 갖춰져 있고 아주 작은 단위로 합칠 수 있는가? > trunk-based
- 릴리스, 핫픽스, 운영 버전을 엄격히 나눠야 하는가? > Git Flow

## merge 방식 비교
- Merge commit: 병합 커밋이 새로 생긴다, 브랜치 작업 흔적이 남는다.
- Squash merge: 여러 커밋을 하나로 합쳐 main에 반영한다.  main 기록이 깔끔해진다.
- Rebase and merge: 커밋을 main 뒤에 이어 붙인다. 기록은 직선에 가깝지만 초보자에게는 어렵다.

## 선택 기준
- 브랜치에서 어떤 커밋들이 있었는지 남기고 싶다. > Merge commit
- 실습 커밋이 여러개라 main 기록을 깔끔하게 남기고 싶다. >  squash merge
- 팀이 Git 기록 정리에 익숙하고 직선형 히스토리를 원한다 > Rebase and merge

## 내가 생각한 협업 규칙
- 작업 전에는 항상 main을 최신화한다.
- main에서 직접 작업하지 않는다.
- PR은 작게 만든다.
- push 전에 `git status`와 변경 내용을 확인한다.
- 커밋 메시지는 누가 봐도 알아볼수 있게 쓰기
- diff 확인하기
- main에 직접 push 하지 않기

## 최종 정리
- Git은 명령어를 외우는 것보다 현재 상태를 확인하면서 안전하게 작업하는 것이 중요하다.