# 🤝 트레블헌터 스터디 기여 가이드

> 이 문서는 스터디 레포에 처음 참여하는 팀원을 위한 단계별 안내서입니다.
> **Git을 처음 써보는 분도 이 문서만 따라오면 첫 기여까지 완료할 수 있어요!**

---

## 📌 목차

1. [사전 준비](#1-사전-준비)
2. [Git과 GitHub 개념 이해](#2-git과-github-개념-이해)
3. [Organization 초대 수락](#3-organization-초대-수락)
4. [첫 기여 실습](#4-첫-기여-실습)
5. [규칙 & 참고사항](#5-규칙--참고사항)

---

## 1. 사전 준비

### 1-1. Git 설치 (Windows)

**① 아래 링크에서 Git 설치 파일 다운로드**

👉 https://git-scm.com/download/win

- 사이트에 들어가면 자동으로 다운로드가 시작돼요
- 파일명이 `Git-x.xx.x-64-bit.exe` 형태면 정상

**② 설치 파일 실행**

설치 중 옵션이 많이 나오는데 **전부 기본값(Next)으로 진행하세요.**
딱 하나만 확인:

```
Choosing the default editor used by Git
→ "Use Visual Studio Code as Git's default editor" 선택 (VS Code 있는 경우)
→ VS Code 없으면 그냥 기본값(Vim)으로 Next
```

**③ 설치 완료 확인**

설치가 끝나면 **Windows 검색창**에 `Git Bash` 검색 → 실행

아래 명령어 입력:
```bash
git --version
```

이렇게 뜨면 설치 성공! ✅
```
git version 2.xx.x.windows.x
```

---

### 1-2. Git 초기 설정

Git Bash를 열고 아래 명령어를 **한 줄씩** 입력하세요.

> ⚠️ 이메일은 반드시 **GitHub 가입 이메일**과 동일해야 해요!
> 다르면 커밋이 본인 기여로 인정되지 않아요.

```bash
git config --global user.name "본인이름"
```
```bash
git config --global user.email "GitHub가입이메일@example.com"
```

**설정 확인:**
```bash
git config --global user.name
git config --global user.email
```

본인 이름과 이메일이 출력되면 성공! ✅

---

### 1-3. PAT (Personal Access Token) 발급

> PAT는 GitHub에 푸시할 때 비밀번호 대신 사용하는 토큰이에요.
> 예전에는 비밀번호로 됐지만, 지금은 반드시 토큰이 필요해요.

**① GitHub 로그인 → 우측 상단 프로필 아이콘 클릭 → Settings**

**② 왼쪽 맨 아래 `Developer settings` 클릭**

**③ `Personal access tokens` → `Tokens (classic)` 클릭**

**④ `Generate new token` → `Generate new token (classic)` 클릭**

**⑤ 아래와 같이 설정:**

| 항목 | 설정값 |
| --- | --- |
| Note | `내 노트북용` (본인이 알아볼 수 있게) |
| Expiration | `90 days` |
| Scopes | `repo` 전체 체크 ✅, `workflow` 체크 ✅ |

**⑥ 맨 아래 `Generate token` 클릭**

**⑦ 생성된 토큰 복사 후 메모장에 저장**

> ⚠️ **지금 복사 안 하면 다시 볼 수 없어요!**
> 창 닫기 전에 반드시 메모장에 붙여넣기 해두세요.

토큰 형태: `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxx`

---

## 2. Git과 GitHub 개념 이해

> 잠깐 읽고 넘어가세요. 나중에 막혔을 때 다시 보면 이해가 돼요.

### Git vs GitHub

| 구분 | 설명 | 비유 |
| --- | --- | --- |
| **Git** | 내 컴퓨터에서 버전을 관리하는 도구 | 개인 일기장 |
| **GitHub** | Git 저장소를 인터넷에 올려 팀과 공유하는 플랫폼 | 공유 구글 드라이브 |

### 브랜치(Branch)란?

> 메인 코드를 건드리지 않고 **내 작업 공간을 따로 만드는 것**

```
main ─────────────────────────────▶ (팀 공용 공간, 보호됨 🔒)
         │
         └─── study/내아이디-day1 ──▶ (내 작업 공간 ✅)
```

- **main 브랜치**: 팀 전체가 공유하는 공간. 직접 수정 불가 (보호됨)
- **내 브랜치**: 내가 자유롭게 작업하는 공간

### PR(Pull Request)란?

> "**내 브랜치의 작업을 main에 합쳐주세요!**" 라고 요청하는 것

```
내 브랜치에서 작업
      ↓
PR 생성 (합쳐달라고 요청)
      ↓
Merge (main에 반영)
      ↓
내 브랜치 삭제 (역할 끝)
```

### 우리 팀 협업 흐름

```
1. 새 브랜치 생성 (study/내아이디-dayN)
2. 브랜치에서 파일 작성
3. 커밋 (저장)
4. PR 생성
5. Merge
6. 브랜치 삭제
```

---

## 3. Organization 초대 수락

> 팀장이 GitHub 이메일로 초대를 보냈어요. 아래 순서로 수락하세요.

**① 가입한 이메일 메일함 확인**

- 발신: `noreply@github.com`
- 제목: `You've been invited to join @travel-hunter on GitHub`
- 스팸함도 꼭 확인!

**② 메일 안의 `Join @travel-hunter` 버튼 클릭**

**③ GitHub 로그인 상태에서 `Accept invitation` 클릭**

**④ 수락 완료 확인**

아래 URL 접속했을 때 레포가 보이면 성공! ✅
```
https://github.com/travel-hunter/travel-hunter-study
```

> ⚠️ 메일을 못 받았다면?
> 팀장에게 GitHub 아이디 알려주고 재초대 요청하세요.

---

## 4. 첫 기여 실습

> **핵심 파트예요!** 천천히 따라오세요.
> 모든 작업은 **GitHub 웹사이트에서** 진행합니다. 터미널 안 써도 돼요!

### 4-1. 레포 접속

👉 https://github.com/travel-hunter/travel-hunter-study

### 4-2. 새 파일 만들기

**① 상단 `Add file` → `Create new file` 클릭**

**② 파일 경로 입력**

파일명 입력칸에 아래 형식으로 입력:
```
members/본인GitHub아이디/2026-04-23-git-basics.md
```

예시:
- GitHub 아이디가 `kimjihoon` → `members/kimjihoon/2026-04-23-git-basics.md`
- GitHub 아이디가 `leechulsoo` → `members/leechulsoo/2026-04-23-git-basics.md`

> 💡 `/` 를 입력하는 순간 폴더가 자동으로 만들어져요!

> ⚠️ 본인 GitHub 아이디 확인 방법:
> GitHub 우측 상단 프로필 클릭 → `Signed in as 아이디` 확인

**③ 파일 내용 입력**

아래 내용을 복사해서 에디터에 붙여넣기:

```markdown
# Git 기초 학습 (2026-04-23)

## 오늘 배운 것
- 

## 궁금한 점
- 

## 다음 학습 예정
- 
```

> 💡 나중에 자유롭게 수정해도 돼요. 지금은 파일 생성이 목적!

### 4-3. 커밋 + 브랜치 생성

**① 우측 상단 `Commit changes...` 클릭**

팝업창이 열려요.

**② Commit message 입력:**
```
docs: 본인아이디 Day 1 git 학습 기록 추가
```

**③ 아래 옵션 선택:**

```
🔒 Commit directly to the main branch  ← 잠겨있음 (선택 불가, 정상이에요!)
✅ Create a new branch for this commit and start a pull request  ← 이거 선택!
```

**④ 브랜치 이름 입력:**

자동으로 이름이 채워지는데, 지우고 아래처럼 입력:
```
study/본인아이디-day1
```
예시: `study/kimjihoon-day1`

**⑤ `Propose changes` 클릭**

### 4-4. Pull Request 생성

자동으로 PR 작성 페이지로 이동해요.

**① 제목 확인** (자동으로 채워져 있어요, 그대로 써도 OK)

**② 본문 입력:**
```markdown
## 📌 변경 내용
- `members/본인아이디/` 폴더 생성
- Day 1 Git 기초 학습 기록 추가

## ✅ 체크리스트
- [x] 폴더 구조 규칙을 따름
- [x] 파일명 규칙(YYYY-MM-DD-주제.md)을 따름
```

**③ `Create pull request` 클릭**

### 4-5. Merge

**① 페이지 아래로 스크롤**

**② `Merge pull request` 버튼 옆 `▼` 클릭 → `Create a merge commit` 선택**

**③ `Merge pull request` → `Confirm merge` 클릭**

### 4-6. 브랜치 삭제

Merge 완료 후 바로 나오는 **`Delete branch`** 버튼 클릭!

> ⚠️ 이거 꼭 해야 해요!
> **"Merge = 브랜치 삭제"** 세트로 기억하세요.

### 4-7. 완료 확인 ✅

아래 URL에서 본인 폴더가 보이면 성공! 🎉
```
https://github.com/travel-hunter/travel-hunter-study/tree/main/members
```

---

## 5. 규칙 & 참고사항

### 커밋 메시지 규칙

```
docs: [주제] 내용 요약

예시:
docs: kimjihoon Day 1 git 학습 기록 추가
docs: [linux] 파일 권한 정리
study: [docker] 컨테이너 실습 결과
```

| prefix | 사용 상황 |
| --- | --- |
| `docs:` | 문서 추가·수정 |
| `study:` | 학습 기록·실습 결과 |

### 파일명 규칙

```
YYYY-MM-DD-주제.md

예시:
2026-04-23-git-basics.md
2026-04-24-linux-commands.md
2026-04-28-docker-intro.md
```

### 브랜치 이름 규칙

```
study/본인아이디-dayN

예시:
study/kimjihoon-day1
study/kimjihoon-day2
```

### ⚠️ 자주 하는 실수

| 실수 | 해결 방법 |
| --- | --- |
| "Commit directly to main" 이 잠겨있어요 | 정상이에요! 아래 "Create a new branch" 선택 |
| 초대 메일을 못 받았어요 | 스팸함 확인 후 팀장에게 재초대 요청 |
| PAT 토큰을 복사 안 했어요 | Settings에서 새 토큰 다시 발급 |
| 브랜치 삭제를 깜빡했어요 | 레포 → branches → 해당 브랜치 옆 🗑️ 아이콘 클릭 |
| 파일명을 한글로 썼어요 | 영문+숫자+하이픈만 사용 (한글은 깨질 수 있음) |

### 막혔을 때

> 이 문서 봐도 모르겠으면 팀 단톡방에 **스크린샷과 함께** 올려주세요!
> 혼자 30분 이상 고민하지 마세요. 바로 물어보는 게 팀 전체에 도움이 돼요. 🙂

---

*last updated: 2026-04-23*
