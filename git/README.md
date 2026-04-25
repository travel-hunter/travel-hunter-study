# Git & GitHub 스터디

Git 및 GitHub 기초 학습 내용을 정리하는 공간입니다.

## 세션 목록

| 세션 | 주제 | 링크 |
|---|---|---|
| Session 1 | Git 기초와 첫 업로드 (add, commit, push, PR) | [session1.md](./session1.md) |
| Session 2 | 브랜치와 PR 실습 (pull, branch, push, PR) | [session2.md](./session2.md) |
| Session 3 | 충돌과 merge 해결 실습 | [session3.md](./session3.md) |
| Session 4 | GitHub Flow와 PR 리뷰 워크플로우 | [session4.md](./session4.md) |
| Session 5 | 실수 복구와 stash 실습 | [session5.md](./session5.md) |
| Session 6 | 브랜치 전략과 협업 운영 정리 | [session6.md](./session6.md) |
| Session 7 | Git 기본기 보강 - init, ignore, diff, log | [session7.md](./session7.md) |

## 경로 기준

- 학습 문서: `git/session1.md` ~ `git/session7.md`
- 롤링페이퍼 충돌 실습 파일: `git/conflict-practice/rolling-paper.md`
- 개인 제출 파일: 저장소 루트의 `members/<본인GitHub아이디>/` 아래에 작성
- 브랜치 규칙: `study/<아이디>-dayN`

`git/members/` 폴더는 만들지 않습니다.

## Session 1-7 전체 흐름

| 세션 | 역할 | 이전 세션과의 연결 | 핵심 실습 |
|---|---|---|---|
| Session 1 | GitHub 제출 흐름 시작 | Git 저장소를 clone하고 첫 제출 흐름을 경험합니다. | `add`, `commit`, `push`, PR 생성 |
| Session 2 | 팀 협업 흐름으로 확장 | 개인 push 이후 `main` 최신화와 새 브랜치 작업을 익힙니다. | `pull`, 브랜치 생성, PR |
| Session 3 | 실제 충돌 재현과 해결 | 여러 PR이 같은 파일을 바꾸면 충돌이 생긴다는 점을 실습합니다. | `git/conflict-practice/rolling-paper.md`, `merge main` |
| Session 4 | PR 품질과 리뷰 문화 | PR을 만드는 단계에서 잘 작성하고 리뷰받는 단계로 확장합니다. | 브랜치 이름, 커밋 메시지, PR 본문, 리뷰 반영 |
| Session 5 | 실수 복구와 작업 보관 | 협업 중 실수했을 때 작업을 되돌리거나 잠시 보관하는 기준을 익힙니다. | `restore`, `revert`, `reset`, `stash` |
| Session 6 | 협업 운영 규칙 정리 | 반복한 협업 흐름을 팀 브랜치 전략과 merge 방식으로 정리합니다. | GitHub Flow, merge 방식, tag, 체크리스트 |
| Session 7 | 기본기 보강 | PR 전 확인 습관을 `diff`, `log`, `.gitignore`로 구체화합니다. | `init`, `.gitignore`, `diff`, `log` |

## 주요 학습 내용
- Git 기본 명령어 (add, commit, push, pull)
- 브랜치 전략
- Pull Request 흐름
- 충돌 해결 (conflict resolution)
- 실수 복구 (restore, revert, reset)
- 작업 임시 보관 (stash)
- GitHub Flow 기반 협업 규칙
- Git 기본기 보강 (init, .gitignore, diff, log)

## 제출 규칙

| 세션 | 제출 파일 | 브랜치 | PR 제목 |
|---|---|---|---|
| Session 1 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session1.md` | `study/<아이디>-day1` | `docs: [git-session1] Git 기초와 첫 업로드` |
| Session 2 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session2.md` | `study/<아이디>-day2` | `docs: [git-session2] 브랜치와 PR 실습` |
| Session 3 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session3.md` | `study/<아이디>-day3` | `docs: [git-session3] 충돌과 merge 해결 실습` |
| Session 4 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session4.md` | `study/<아이디>-day4` | `docs: [git-session4] GitHub Flow와 PR 리뷰 워크플로우` |
| Session 5 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session5.md` | `study/<아이디>-day5` | `docs: [git-session5] 실수 복구와 stash 실습` |
| Session 6 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session6.md` | `study/<아이디>-day6` | `docs: [git-session6] 브랜치 전략과 협업 운영 정리` |
| Session 7 | `members/<본인GitHub아이디>/YYYY-MM-DD-git-session7.md` | `study/<아이디>-day7` | `docs: [git-session7] Git 기본기 보강 - init, ignore, diff, log` |
