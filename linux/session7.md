# Session 7. vim & 실전 배포 시나리오

> 실습 환경: VMware + Ubuntu Server 24.04 + MobaXterm SSH

## 학습 목표

- `vim`으로 서버에서 설정 파일과 코드 파일을 수정할 수 있다
- Python FastAPI 앱을 Linux 서버에서 실행할 수 있다
- `.env`, `requirements.txt`, `systemd`를 배포 흐름에 연결할 수 있다
- 앱이 실행되지 않을 때 로그, 포트, 권한, 환경 변수를 순서대로 점검할 수 있다
- 지금까지 배운 Linux 명령어를 앱 배포 시나리오에 적용할 수 있다

---

## 1. vim — 서버에서 파일 편집하기

### 1.1 vim이 필요한 이유

서버에는 보통 VS Code나 메모장 같은 GUI 기반 편집기가 없다. 따라서 서버에서 설정 파일을 수정할 때는 터미널에서 동작하는 편집기를 사용한다. 가장 널리 쓰이는 도구가 `vim`이다.

앱 배포 중 vim으로 자주 수정하는 파일:

| 파일 | 용도 |
| --- | --- |
| `.env` | 환경 변수 설정 |
| `requirements.txt` | Python 패키지 목록 |
| `main.py` | Python 앱 코드 |
| `서비스명.service` | systemd 서비스 설정 |
| `nginx.conf` | nginx 설정 |

### 1.2 vim의 두 가지 모드

vim은 모드 개념이 핵심이다.

| 모드 | 설명 | 진입 방법 |
| --- | --- | --- |
| 명령 모드 | 저장, 종료, 삭제, 검색 가능 | `ESC` |
| 입력 모드 | 글자 입력 가능 | `i` |

> **헷갈릴 때**
> 입력이 안 되면 `i`를 누른다.
> 저장이나 종료가 안 되면 `ESC`를 먼저 누른다.

### 1.3 vim 필수 명령어

| 키 | 동작 |
| --- | --- |
| `vim 파일명` | 파일 열기 |
| `i` | 입력 모드 진입 |
| `ESC` | 명령 모드로 돌아가기 |
| `:w` | 저장 |
| `:q` | 종료 |
| `:wq` | 저장 후 종료 |
| `:q!` | 저장하지 않고 강제 종료 |
| `dd` | 한 줄 삭제 |
| `yy` | 한 줄 복사 |
| `p` | 붙여넣기 |
| `u` | 실행 취소 |
| `/검색어` | 문자열 검색 |
| `n` | 다음 검색 결과 |
| `G` | 파일 맨 아래로 이동 |
| `gg` | 파일 맨 위로 이동 |
| `:숫자` | 특정 줄로 이동 |

### 실습 1. vim으로 파일 만들기

**1단계. 실습 디렉토리 생성**

```bash
mkdir ~/vim_practice
cd ~/vim_practice
```

**2단계. vim으로 파일 열기**

```bash
vim memo.txt
```

**3단계. 입력 모드 진입**

`i`를 누른 뒤 다음 내용을 입력한다.

```
Linux deployment practice
vim test file
```

**4단계. 저장 후 종료**

`ESC`를 누른 뒤 `:wq` 입력.

**5단계. 파일 확인**

```bash
cat memo.txt
```

---

## 2. 실전 시나리오 — FastAPI 앱 배포 흐름

지금까지 배운 내용을 하나로 연결하는 실습이다. 목표는 Ubuntu 서버에서 FastAPI 앱을 실행하고, `systemd` 서비스로 등록하는 것이다.

전체 흐름:

```
패키지 설치 → 프로젝트 디렉토리 생성 → 가상환경 생성 → 패키지 설치
→ .env 작성 → 앱 실행 테스트 → systemd 등록 → 서비스 실행 → curl 테스트
```

### 실습 2. 배포 기본 패키지 설치

**1단계. 패키지 목록 최신화**

```bash
sudo apt update
```

**2단계. 필요한 패키지 설치**

```bash
sudo apt install python3 python3-pip python3-venv git curl -y
```

**3단계. 설치 확인**

```bash
python3 --version
pip3 --version
git --version
curl --version
```

### 실습 3. FastAPI 프로젝트 만들기

**1단계. 프로젝트 디렉토리 생성**

```bash
mkdir -p ~/myapp/travelhunter
cd ~/myapp/travelhunter
```

**2단계. Python 가상환경 생성**

```bash
python3 -m venv venv
```

**3단계. 가상환경 활성화**

```bash
source venv/bin/activate
```

성공하면 터미널 프롬프트 앞에 `(venv)`가 표시된다.

**4단계. FastAPI 패키지 설치**

```bash
pip install fastapi uvicorn python-dotenv
```

**5단계. requirements.txt 생성**

```bash
pip freeze > requirements.txt
```

**6단계. requirements.txt 확인**

```bash
cat requirements.txt
```

### 실습 4. FastAPI 앱 파일 만들기

**1단계. main.py 생성**

```bash
vim main.py
```

내용 입력:

```python
import os
from dotenv import load_dotenv
from fastapi import FastAPI

load_dotenv()

app = FastAPI()

@app.get("/")
def home():
    return {
        "message": "TravelHunter API is running",
        "env": os.getenv("APP_ENV")
    }

@app.get("/health")
def health():
    return {
        "status": "ok"
    }
```

**2단계. .env 파일 생성**

```bash
vim .env
```

내용 입력:

```bash
APP_ENV=development
APP_NAME=travelhunter
```

**3단계. .env 권한 설정**

```bash
chmod 600 .env
```

**4단계. .gitignore 생성**

```bash
vim .gitignore
```

내용 입력:

```
.env
venv/
__pycache__/
```

### 실습 5. 앱 직접 실행 테스트

**1단계. 앱 실행**

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

> 이 명령어를 실행하면 해당 터미널은 앱이 점유한다. 다음 단계는 **새 터미널에서** 진행해야 한다. MobaXterm에서는 좌측 세션 패널의 동일 세션을 다시 더블클릭하면 새 탭이 열린다.

**2단계. (새 터미널) 포트 확인**

```bash
ss -tulpn | grep 8000
```

**3단계. (새 터미널) curl로 접속 확인**

```bash
curl http://localhost:8000
```

**4단계. (새 터미널) health API 확인**

```bash
curl http://localhost:8000/health
```

정상 출력 예시:

```bash
{"status":"ok"}
```

**5단계. 앱 종료**

앱을 실행한 첫 번째 터미널에서 `Ctrl + C`를 누른다.

---

## 3. systemd 서비스 등록

### 3.1 systemd가 필요한 이유

터미널에서 직접 실행한 앱은 터미널을 닫으면 함께 종료된다. 하지만 배포 서버에서는 앱이 백그라운드에서 계속 실행되어야 한다. 이때 `systemd` 서비스를 사용한다.

systemd로 가능한 작업:

| 작업 | 설명 |
| --- | --- |
| 서비스 시작 | 앱 실행 |
| 서비스 중지 | 앱 종료 |
| 서비스 재시작 | 앱 다시 실행 |
| 자동 시작 | 서버 재부팅 후 자동 실행 |
| 로그 확인 | 앱 실행 로그 확인 |

### 실습 6. systemd 서비스 파일 만들기

**1단계. 서비스 파일 생성**

```bash
sudo vim /etc/systemd/system/travelhunter.service
```

**2단계. 서비스 파일 내용 입력**

```ini
[Unit]
Description=TravelHunter FastAPI App
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/myapp/travelhunter
EnvironmentFile=/home/ubuntu/myapp/travelhunter/.env
ExecStart=/home/ubuntu/myapp/travelhunter/venv/bin/uvicorn main:app --host 0.0.0.0 --port 8000
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

> **주의**
> 사용자명이 `ubuntu`가 아니라면 `whoami` 명령어로 확인한 사용자명으로 바꿔야 한다.

**3단계. 현재 사용자 확인**

```bash
whoami
```

결과가 `ubuntu`가 아니라면 서비스 파일의 다음 부분을 수정한다.

```
User=ubuntu
```

### 실습 7. 서비스 시작하기

**1단계. systemd 설정 다시 읽기**

```bash
sudo systemctl daemon-reload
```

**2단계. 서비스 시작**

```bash
sudo systemctl start travelhunter
```

**3단계. 서비스 상태 확인**

```bash
sudo systemctl status travelhunter
```

다음 메시지가 보이면 정상이다.

```
active (running)
```

**4단계. 서버 재부팅 후 자동 실행 설정**

```bash
sudo systemctl enable travelhunter
```

**5단계. curl로 접속 확인**

```bash
curl http://localhost:8000/health
```

### 실습 8. 서비스 제어하기

```bash
# 서비스 중지
sudo systemctl stop travelhunter

# 서비스 시작
sudo systemctl start travelhunter

# 서비스 재시작
sudo systemctl restart travelhunter

# 서비스 상태 확인
sudo systemctl status travelhunter

# 서비스 로그 확인
sudo journalctl -u travelhunter -n 50

# 서비스 로그 실시간 확인 (Ctrl + C로 종료)
sudo journalctl -u travelhunter -f
```

---

## 4. 배포 디버깅 흐름

앱이 실행되지 않을 때는 다음 순서로 점검한다.

### 4.1 서비스 상태 확인

```bash
sudo systemctl status travelhunter
```

| 상태 | 의미 |
| --- | --- |
| `active (running)` | 정상 실행 중 |
| `failed` | 실행 실패 |
| `inactive` | 실행 중이 아님 |

### 4.2 로그 확인

```bash
sudo journalctl -u travelhunter -n 50
```

실시간 로그:

```bash
sudo journalctl -u travelhunter -f
```

로그에서 자주 보는 에러:

| 에러 | 의미 |
| --- | --- |
| `ModuleNotFoundError` | Python 패키지 설치 안 됨 |
| `No such file or directory` | 경로가 틀림 |
| `Permission denied` | 권한 문제 |
| `Address already in use` | 포트가 이미 사용 중 |
| `EnvironmentFile` | `.env` 경로 문제 |

### 4.3 포트 확인

```bash
ss -tulpn | grep 8000
```

8000번 포트가 보이면 앱이 해당 포트에서 실행 중인 것이다.

### 4.4 포트가 이미 사용 중일 때

```bash
sudo lsof -i :8000
```

PID 확인 후 종료:

```bash
kill {PID}
```

강제 종료:

```bash
kill -9 {PID}
```

### 4.5 경로 확인

서비스 파일에 적은 경로가 실제로 존재하는지 확인한다.

```bash
ls -la /home/ubuntu/myapp/travelhunter
ls -la /home/ubuntu/myapp/travelhunter/venv/bin/uvicorn
ls -la /home/ubuntu/myapp/travelhunter/.env
```

### 4.6 환경 변수 확인

```bash
cat .env
```

`.env` 권한 확인:

```bash
ls -l .env
```

권장 권한: `-rw-------`

권한 수정:

```bash
chmod 600 .env
```

### 4.7 Python 패키지 확인

가상환경을 활성화한 뒤 확인한다.

```bash
cd ~/myapp/travelhunter
source venv/bin/activate
pip list
```

필요하면 다시 설치한다.

```bash
pip install -r requirements.txt
```

### 4.8 직접 실행 테스트

systemd 문제가 의심되면 먼저 직접 실행해본다.

```bash
cd ~/myapp/travelhunter
source venv/bin/activate
uvicorn main:app --host 0.0.0.0 --port 8000
```

직접 실행은 되는데 systemd만 실패한다면 서비스 파일의 경로, 사용자명, 환경 변수 설정을 확인한다.

---

## 5. 통합 치트시트

### 파일·디렉토리

```bash
pwd
ls -la
cd 경로
mkdir -p 폴더
touch 파일
cp 원본 대상
cp -r 원본폴더 대상폴더
mv 원본 대상
rm 파일
rm -r 폴더
cat 파일
tail -f 파일
```

### 권한

```bash
chmod 644 파일
chmod 755 파일
chmod 600 .env
chmod 600 키파일.pem
chown 사용자:사용자 파일
sudo 명령어
```

### 패키지

```bash
sudo apt update
sudo apt install 패키지명 -y
sudo apt remove 패키지명
apt list --installed | grep 패키지명
```

### Python 배포

```bash
python3 -m venv venv
source venv/bin/activate
deactivate
pip install 패키지명
pip install -r requirements.txt
pip freeze > requirements.txt
pip list
```

### 환경 변수

```bash
printenv
echo $변수명
export 변수명=값
echo ".env" >> .gitignore
```

### 프로세스·포트

```bash
ps -ef | grep 이름
kill PID
kill -9 PID
ss -tulpn
ss -tulpn | grep 8000
sudo lsof -i :8000
```

### systemd

```bash
sudo systemctl daemon-reload
sudo systemctl start 서비스명
sudo systemctl stop 서비스명
sudo systemctl restart 서비스명
sudo systemctl status 서비스명
sudo systemctl enable 서비스명
sudo journalctl -u 서비스명 -n 50
sudo journalctl -u 서비스명 -f
```

### 네트워크 테스트

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
curl http://localhost:8000
curl http://localhost:8000/health
curl -I http://localhost:8000
```

### vim

```
vim 파일명
i
ESC
:w
:q
:wq
:q!
dd
yy
p
u
/검색어
```

### 시스템 점검

```bash
df -h
du -sh *
free -h
top
uptime
hostname
whoami
```

---

## 마지막 과제

1. `~/myapp/travelhunter` 디렉토리에 FastAPI 앱을 만들고 실행한다.
2. `curl http://localhost:8000/health` 명령어로 앱 응답을 확인한다.
3. `.env` 파일을 만들고 Python 코드에서 `APP_ENV` 값을 읽어본다.
4. `requirements.txt` 파일을 생성한다.
5. `travelhunter.service` 파일을 만들어 systemd 서비스로 등록한다.
6. `sudo systemctl status travelhunter`로 서비스 상태를 확인한다.
7. `sudo journalctl -u travelhunter -n 50`으로 로그를 확인한다.
8. 본인이 자주 쓸 것 같은 Linux 명령어 20개를 골라 개인 치트시트를 만들어본다.

---

## 최종 목표

이번 Linux 기초 과정의 최종 목표는 명령어를 많이 외우는 것이 아니다. 앱을 배포할 때 다음 흐름을 스스로 따라갈 수 있으면 충분하다.

```
서버 접속 → 패키지 설치 → 코드 준비 → 가상환경 생성
→ 패키지 설치 → 환경 변수 설정 → 앱 실행 → 서비스 등록
→ 로그 확인 → curl 테스트 → 문제 발생 시 디버깅
```