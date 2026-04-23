# 기초 학습 (2026-04-23 ~ )


## 2026-04-23 : 오늘 배운 것

### git add & git commit
인과 흐름
작업폴더-> git add 파일명 -> staging (area) -> git commit -m '메모' -> repository & 메모

- staging : git status로 확인
- repository : git log로 확인


### git diff & git difftool & 그외 직관적인 방법들
인과 흐름
git diff or git difftool -> 이전 commit 코드 vs 수정된 코드 비교

- 커밋ID 확인 : git log --oneline --all
	- git diff 커밋ID
        - git difftool 커밋ID 1 커밋 ID 2

##### 그외 직관적인 방법들
- git graph extension 활용
- source control -> [+ / -] ->staging / repository

------
## 다음 학습 예정
