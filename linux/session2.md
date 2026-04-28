# Session 2. 권한 관리 & sudo

> 실습 환경: VMware + Ubuntu Server 24.04 + MobaXterm (SSH)

## 이번 세션 목표

- `Permission denied` 에러의 원인을 이해한다.
- 파일·디렉토리 권한을 읽고 변경할 수 있다.
- 대표 권한 조합(755, 644, 600)을 익힌다.
- `sudo`로 관리자 작업을 수행할 수 있다.

---

## 1. 왜 권한이 필요한가

Linux는 다중 사용자 시스템으로 설계되었기 때문에, 모든 파일에는 **누가 무엇을 할 수 있는지**가 정의되어 있다. 배포 과정에서 마주치는 `Permission denied` 에러의 대부분은 권한 문제다.

Ubuntu Server는 보안상 root 계정이 기본적으로 비활성화되어 있으며, 모든 관리자 작업은 `sudo`를 통해 수행한다. 이는 실무 표준이다.

---

## 2. 권한 읽기

### 2.1 `ls -l` 출력 분석

```bash
ls -l
```

출력 예시:

```
-rwxr-xr-- 1 ubuntu ubuntu 1234 Apr 25 10:00 app.sh
```

| 부분 | 의미 |
| --- | --- |
| `-` | 파일 유형 (`-` 파일, `d` 디렉토리, `l` 링크) |
| `rwx` | 소유자(user) 권한 |
| `r-x` | 그룹(group) 권한 |
| `r--` | 기타(other) 권한 |
| `1` | 링크 수 |
| `ubuntu` | 소유자 |
| `ubuntu` | 그룹 |
| `1234` | 파일 크기 (byte) |
| `Apr 25 10:00` | 최종 수정 시각 |
| `app.sh` | 파일명 |

### 2.2 권한 문자

| 문자 | 의미 | 파일에서 | 디렉토리에서 |
| --- | --- | --- | --- |
| `r` | read | 내용 보기 | 목록 보기 |
| `w` | write | 내용 수정 | 파일 추가·삭제 |
| `x` | execute | 실행 | 진입(`cd`) |
| `-` | 권한 없음 | | |

### 2.3 권한 위치

```
-rwxr-xr--
 ↑↑↑↑↑↑↑↑↑
 │└─┘└─┘└─┘
 │ │  │  └─ 기타 사용자 (other)
 │ │  └──── 그룹 (group)
 │ └─────── 소유자 (user/owner)
 └───────── 파일 유형
```

> MobaXterm 좌측 SFTP 패널에서도 파일별 권한을 시각적으로 확인할 수 있다.

---

## 3. 숫자 표기법

각 권한은 숫자로 환산해 합산한다.

| 권한 | 숫자 |
| --- | --- |
| r (읽기) | 4 |
| w (쓰기) | 2 |
| x (실행) | 1 |

자주 쓰는 조합:

| 숫자 | 권한 | 설명 | 사용처 |
| --- | --- | --- | --- |
| `755` | `rwxr-xr-x` | 소유자는 모든 권한, 그룹·기타는 읽기와 실행 | 실행 파일·스크립트·디렉토리 |
| `644` | `rw-r--r--` | 소유자는 읽기·쓰기, 그룹·기타는 읽기 | 일반 텍스트 파일 |
| `600` | `rw-------` | 소유자만 읽기·쓰기 | 비밀 파일 (SSH 키, `.env`) |
| `700` | `rwx------` | 소유자만 모든 권한 | 개인 전용 디렉토리 |
| `777` | `rwxrwxrwx` | 모두 모든 권한 | 보안상 권장하지 않음 |

계산 예시: `755 = 7(4+2+1) + 5(4+1) + 5(4+1) = rwxr-xr-x`

---

## 4. 권한 변경 — `chmod`

### 4.1 숫자 방식

```bash
sudo chmod 755 deploy.sh        # 실행 가능한 스크립트
sudo chmod 644 readme.txt       # 일반 파일
sudo chmod 600 my-key.pem       # SSH 키 (EC2 접속 시 필수)
sudo chmod 700 ~/secret_folder  # 개인 디렉토리
```

### 4.2 문자 방식

| 기호 | 의미 |
| --- | --- |
| `u` | user (소유자) |
| `g` | group (그룹) |
| `o` | other (기타) |
| `a` | all (모두) |
| `+` | 권한 추가 |
| `-` | 권한 제거 |
| `=` | 권한 설정 |

```bash
sudo chmod +x run.sh            # 모두에게 실행 권한 추가
sudo chmod u+w file.txt         # 소유자에게 쓰기 권한 추가
sudo chmod o-r secret.txt       # 기타 사용자의 읽기 권한 제거
sudo chmod a=r public.txt       # 모두에게 읽기 권한만 부여
```

### 4.3 디렉토리 전체 적용 — `-R`

```bash
sudo chmod -R 755 myproject/    # 하위 모든 파일·디렉토리에 적용
```

### 실습 1. chmod 사용해보기

```bash
# 1. 연습용 디렉토리와 파일 생성
mkdir ~/permission_test
cd ~/permission_test
echo "Hello" > test.txt

# 2. 현재 권한 확인
ls -l test.txt

# 3. 권한을 600으로 변경
sudo chmod 600 test.txt
ls -l test.txt
# -rw------- 인지 확인

# 4. 다시 644로 변경
sudo chmod 644 test.txt
ls -l test.txt

# 5. 실행 권한 추가
sudo chmod +x test.txt
ls -l test.txt

# 6. 정리
cd ~
rm -rf permission_test
```

---

## 5. 소유자 변경 — `chown`

### 왜 필요한가

- root가 만든 파일은 일반 유저가 수정할 수 없다
- 배포 과정에서 권한·소유권이 꼬이는 경우가 잦다

### 5.1 사전 준비: 새 사용자 생성

```bash
sudo adduser devuser
```

비밀번호를 설정하고 엔터를 몇 번 누르면 생성된다.

확인:

```bash
id devuser
```

#### `useradd`와 `adduser`의 차이

- `useradd`: 최소한의 설정만 처리한다. 홈 디렉토리·셸 설정이 자동 생성되지 않으므로 추가 작업이 필요하다.
- `adduser`: 홈 디렉토리 생성, 셸 설정, 그룹 멤버십 설정까지 자동으로 처리하는 대화형 명령어다. 일반적으로 더 편리하다.

### 5.2 실습용 디렉토리 생성

```bash
mkdir ~/app
cd ~/app
touch test.txt
ls -l
```

다음과 같이 보일 것이다:

```
-rw-r--r-- 1 ubuntu ubuntu 0 test.txt
```

### 5.3 소유자 변경

먼저 `devuser`로 접속해 파일 수정을 시도해본 뒤, 다음 명령어들을 차례로 실행한다.

```bash
# ubuntu 계정에서 실행
sudo chown devuser test.txt            # 소유자만 변경
ls -l
sudo chown devuser:devuser test.txt    # 소유자와 그룹 함께 변경
sudo chown -R devuser:devuser ~/app    # 디렉토리 전체 변경
```

### 5.4 devuser로 접속해 테스트

```bash
su - devuser
```

파일 수정 시도:

```bash
echo "hello" >> /home/ubuntu/app/test.txt
```

> 이 명령이 실패한다면 어떤 문제 때문일까?
> 오늘 배운 내용과 관련 있으니 직접 해결해보자.
>
> <details>
> <summary>힌트 보기</summary>
>
> 파일에 접근하려면 모든 상위 디렉토리에 `x` 권한이 필요하다.
>
> </details>

### 5.5 devuser 정리

```bash
# 1. 파일 소유권 원복
sudo chown -R ubuntu:ubuntu /home/ubuntu/app

# 2. devuser 삭제 (홈 디렉토리 포함)
sudo userdel -r devuser

# 3. 삭제 확인
id devuser

# 4. 남은 파일 확인
sudo find / -user devuser
```

---

## 6. sudo — 관리자 권한

### 6.1 sudo란

**S**uper **U**ser **DO**의 약자로, 관리자(root) 권한으로 명령어를 실행한다.

```bash
sudo systemctl restart nginx    # 서비스 재시작
sudo cat /var/log/auth.log      # 관리자만 볼 수 있는 파일 읽기
```

### 6.2 자주 쓰는 패턴

```bash
sudo -i      # root 계정으로 전환 (exit으로 빠져나오기)
sudo su -    # 위와 유사
sudo !!      # 방금 친 명령어를 sudo로 재실행
```

> `sudo !!`는 명령어를 친 후 `Permission denied`가 떴을 때 매우 유용하다. 같은 명령어를 sudo로 자동 재실행해준다.

### 6.3 sudo 비밀번호

- `sudo`를 처음 사용할 때 비밀번호를 묻는다
- **본인 계정의 비밀번호**를 입력한다 (root 비밀번호 아님)
- 한 번 입력하면 일정 시간(기본 15분) 동안 다시 묻지 않는다

### 6.4 Ubuntu Server에서의 sudo 그룹

설치 시 만든 첫 번째 사용자(`ubuntu`)는 자동으로 sudo 그룹에 포함된다.

```bash
groups
# 출력 예: ubuntu adm cdrom sudo dip ...
# sudo가 포함되어 있으면 OK
```

---

## 7. 실전 시나리오

배포 디렉토리 권한 설정을 시뮬레이션한다.

```bash
# 1. 가상 배포 디렉토리 생성
sudo mkdir -p /var/www/myapp
sudo touch /var/www/myapp/index.html
sudo touch /var/www/myapp/secret.env

# 2. 현재 소유자 확인 (모두 root 소유)
ls -l /var/www/myapp

# 3. 소유자를 ubuntu로 변경
sudo chown -R ubuntu:ubuntu /var/www/myapp

# 4. 일반 파일은 644로
sudo chmod 644 /var/www/myapp/index.html

# 5. 비밀 파일은 600으로
sudo chmod 600 /var/www/myapp/secret.env

# 6. 결과 확인
ls -l /var/www/myapp

# 7. 정리
sudo rm -rf /var/www/myapp
```

---

## 8. 명령어 요약

| 명령어 | 동작 |
| --- | --- |
| `ls -l` | 권한 확인 |
| `chmod 755 파일` | 실행 가능한 스크립트 권한 |
| `chmod 644 파일` | 일반 파일 권한 |
| `chmod 600 파일` | 비밀 파일 권한 (`.pem`, `.env`) |
| `chmod +x 파일` | 실행 권한 추가 |
| `chmod -R 755 폴더/` | 디렉토리 전체 적용 |
| `chown 사용자:그룹 파일` | 소유자 변경 |
| `sudo 명령어` | 관리자 권한으로 실행 |
| `sudo !!` | 직전 명령어 sudo로 재실행 |
| `sudo -i` | root로 전환 |
| `groups` | 내가 속한 그룹 확인 |

---

## 과제

1. 홈 디렉토리에 `secret.txt` 파일을 만들고 권한을 `600`으로 설정한다. `ls -l`로 `rw-------`가 표시되는지 확인한다.
2. `chmod 755`와 `chmod 644`의 차이를 본인의 말로 정리해 git에 공유한다.
3. `sudo !!`를 직접 사용해본다.
   예: `apt update`를 실행 → `Permission denied` 발생 → `sudo !!`로 재실행