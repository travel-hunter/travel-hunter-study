# Session 6. 패키지 관리 & 환경 변수

> 실습 환경: VMware + Ubuntu Server 24.04 + MobaXterm (SSH)

## 학습 목표

- Ubuntu에서 `apt`로 배포에 필요한 패키지를 설치할 수 있다
- Python 가상환경을 만들고 패키지를 설치할 수 있다
- `requirements.txt`로 Python 실행 환경을 저장하고 복원할 수 있다
- 환경 변수로 비밀번호, API 키, DB 주소를 관리할 수 있다
- `.env`, `.env.example`, `.gitignore`의 역할을 이해한다
- `.bashrc`로 자주 쓰는 환경 변수를 영구 설정할 수 있다

---

## 1. 앱 배포에서 패키지가 필요한 이유

앱을 서버에 배포하려면 필요한 프로그램을 설치해야 한다. 예를 들어 Python 백엔드 앱을 배포한다면 다음과 같은 패키지가 필요할 수 있다.

| 패키지 | 용도 |
| --- | --- |
| `python3` | Python 실행 |
| `python3-pip` | Python 패키지 설치 |
| `python3-venv` | Python 가상환경 생성 |
| `git` | GitHub에서 코드 가져오기 |
| `curl` | 서버 응답 테스트 |
| `nginx` | 웹 서버 / 리버스 프록시 |
| `unzip` | 압축 파일 해제 |

패키지는 직접 만드는 것이 아니라, 이미 만들어진 프로그램을 서버에 설치해 사용하는 것이다.

---

## 2. Ubuntu 패키지 관리 — apt

### 2.1 apt란

`apt`는 Ubuntu에서 패키지를 설치, 삭제, 업데이트할 때 사용하는 패키지 관리자다.

### 2.2 패키지 목록 최신화

패키지를 설치하기 전에 먼저 실행한다.

```bash
sudo apt update
```

> `apt update`는 프로그램을 업데이트하는 명령이 아니라, 설치 가능한 패키지 목록을 최신화하는 명령이다.

### 2.3 패키지 설치

```bash
sudo apt install 패키지명
```

예시:

```bash
sudo apt install git
sudo apt install curl
sudo apt install nginx
```

설치할 때 확인을 묻지 않게 하려면 `-y` 옵션을 붙인다.

```bash
sudo apt install nginx -y
```

### 2.4 패키지 삭제

```bash
sudo apt remove 패키지명
```

예시:

```bash
sudo apt remove nginx
```

설정 파일까지 함께 삭제하려면 `purge`를 사용한다.

```bash
sudo apt purge nginx
```

### 2.5 설치된 패키지 확인

```bash
apt list --installed | grep nginx
```

### 실습 1. 배포 기본 패키지 설치

**1단계. 패키지 목록 최신화**

```bash
sudo apt update
```

**2단계. 기본 패키지 설치**

```bash
sudo apt install git curl unzip python3 python3-pip python3-venv -y
```

**3단계. 설치 확인**

```bash
git --version
curl --version
python3 --version
pip3 --version
```

---

## 3. nginx 설치와 실행 확인

### 3.1 nginx란

`nginx`는 웹 서버다. 앱 배포에서는 보통 다음 역할로 사용한다.

| 역할 | 설명 |
| --- | --- |
| 정적 파일 제공 | HTML, CSS, JS 파일 제공 |
| 리버스 프록시 | 외부 요청을 백엔드 앱으로 전달 |
| 포트 연결 | 80, 443 포트로 들어온 요청을 앱 포트로 전달 |

이번 실습에서는 nginx를 설치하고 실행 상태만 확인한다.

### 실습 2. nginx 설치 확인

앞에서 이미 설치했다면, 켜져 있는지 확인만 하고 넘어간다.

**1단계. nginx 설치**

```bash
sudo apt update
sudo apt install nginx -y
```

**2단계. nginx 상태 확인**

```bash
systemctl status nginx
```

다음 메시지가 보이면 정상이다.

```bash
active (running)
```

**3단계. 로컬에서 접속 확인**

```bash
curl localhost
```

HTML이 출력되면 nginx가 정상 실행 중이다.

**4단계. nginx 제어 명령어**

```bash
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl status nginx
```

---

## 4. Python 가상환경

### 4.1 왜 가상환경이 필요한가

프로젝트마다 사용하는 Python 패키지가 다를 수 있다.

| 프로젝트 | 필요한 패키지 |
| --- | --- |
| 프로젝트 A | FastAPI 0.100 |
| 프로젝트 B | FastAPI 0.110 |
| 프로젝트 C | Django |

모든 패키지를 서버 전체에 설치하면 충돌이 생길 수 있다. 그래서 프로젝트별로 독립된 Python 공간을 만든다. 이것이 **가상환경**이다.

### 실습 3. Python 가상환경 만들기

**1단계. 실습 디렉토리 생성**

```bash
mkdir ~/python_app_test
cd ~/python_app_test
```

**2단계. 가상환경 생성**

```bash
python3 -m venv venv
```

**3단계. 가상환경 활성화**

```bash
source venv/bin/activate
```

성공하면 터미널 앞에 `(venv)`가 표시된다.

```bash
(venv) ubuntu@server:~/python_app_test$
```

**4단계. Python 패키지 설치**

```bash
pip install requests
```

**5단계. 설치된 패키지 확인**

```bash
pip list
```

**6단계. 가상환경 종료**

```bash
deactivate
```

---

## 5. requirements.txt

### 5.1 requirements.txt란

`requirements.txt`는 Python 프로젝트에 필요한 패키지 목록을 저장하는 파일이다. 배포할 때 서버에서 동일한 패키지를 다시 설치하기 위해 사용한다.

### 5.2 현재 설치된 패키지 저장

가상환경이 활성화된 상태에서 실행한다.

```bash
pip freeze > requirements.txt
```

### 5.3 requirements.txt 확인

```bash
cat requirements.txt
```

출력 예시:

```bash
requests==2.32.3
```

### 5.4 다른 서버에서 같은 패키지 설치

```bash
pip install -r requirements.txt
```

### 실습 4. requirements.txt 만들기

**1단계. 가상환경 활성화**

```bash
cd ~/python_app_test
source venv/bin/activate
```

**2단계. 패키지 설치**

```bash
pip install requests python-dotenv
```

**3단계. 패키지 목록 저장**

```bash
pip freeze > requirements.txt
```

**4단계. 파일 확인**

```bash
cat requirements.txt
```

---

## 6. 환경 변수

### 6.1 환경 변수란

환경 변수는 서버나 프로그램이 사용할 설정값이다. 앱 배포에서는 다음과 같은 값을 환경 변수로 관리한다.

| 값 | 예시 |
| --- | --- |
| DB 주소 | `DB_HOST=localhost` |
| DB 사용자 | `DB_USER=admin` |
| DB 비밀번호 | `DB_PASSWORD=secret123` |
| API 키 | `API_KEY=abcd1234` |
| 실행 모드 | `APP_ENV=production` |

비밀번호나 API 키를 코드에 직접 작성하면 GitHub에 올라갈 위험이 있다.

### 6.2 환경 변수 확인

```bash
printenv
```

특정 환경 변수만 확인:

```bash
echo $HOME
echo $USER
echo $PATH
```

### 6.3 임시 환경 변수 설정

```bash
export DB_PASSWORD=secret123
```

확인:

```bash
echo $DB_PASSWORD
```

> `export`로 설정한 환경 변수는 현재 터미널에서만 유지된다. 터미널을 닫거나 새로 접속하면 사라진다.

### 6.4 Python에서 환경 변수 읽기

```python
import os

db_password = os.getenv("DB_PASSWORD")
print(db_password)
```

### 실습 5. 환경 변수 사용하기

**1단계. 환경 변수 설정**

```bash
export DB_PASSWORD=secret123
```

**2단계. Python 파일 생성**

```bash
vim env_test.py
```

내용 입력:

```python
import os

db_password = os.getenv("DB_PASSWORD")
print(db_password)
```

**3단계. 실행**

```bash
python3 env_test.py
```

출력:

```bash
secret123
```

---

## 7. .env 파일

### 7.1 .env 파일이란

`.env` 파일은 프로젝트에서 사용할 환경 변수를 모아두는 파일이다.

예시:

```bash
DB_HOST=localhost
DB_USER=admin
DB_PASSWORD=secret123
APP_ENV=development
```

개발 단계에서 `.env` 파일을 자주 사용한다.

### 7.2 .env 파일 주의사항

`.env`에는 비밀번호와 API 키가 들어갈 수 있으므로 GitHub에 올리면 안 된다. 반드시 `.gitignore`에 추가한다.

```bash
echo ".env" >> .gitignore
```

### 7.3 .env.example

`.env` 파일은 공유하지 않지만, 어떤 값이 필요한지는 팀원에게 알려줘야 한다. 그래서 `.env.example` 파일을 만든다.

```bash
DB_HOST=
DB_USER=
DB_PASSWORD=
APP_ENV=
```

`.env.example`에는 실제 비밀번호를 작성하지 않는다.

### 실습 6. .env 파일 사용하기

**1단계. 실습 디렉토리 생성**

```bash
mkdir ~/env_test
cd ~/env_test
```

**2단계. 가상환경 생성 및 활성화**

```bash
python3 -m venv venv
source venv/bin/activate
```

**3단계. python-dotenv 설치**

```bash
pip install python-dotenv
```

**4단계. .env 파일 생성**

```bash
vim .env
```

내용 입력:

```bash
PASSWORD=1234
APP_ENV=development
```

**5단계. Python 파일 생성**

```bash
vim test.py
```

내용 입력:

```python
import os
from dotenv import load_dotenv

load_dotenv()

print(os.getenv("PASSWORD"))
print(os.getenv("APP_ENV"))
```

**6단계. 실행**

```bash
python test.py
```

출력:

```bash
1234
development
```

**7단계. .gitignore 생성**

```bash
echo ".env" >> .gitignore
```

**8단계. .env.example 생성**

```bash
vim .env.example
```

내용 입력:

```bash
PASSWORD=
APP_ENV=
```

---

## 8. .bashrc

### 8.1 .bashrc란

`.bashrc`는 사용자가 터미널에 접속할 때 자동으로 실행되는 설정 파일이다. 자주 쓰는 환경 변수나 단축 명령어를 저장할 수 있다.

### 8.2 .bashrc에 환경 변수 추가

```bash
vim ~/.bashrc
```

파일 맨 아래에 추가:

```bash
export APP_ENV=development
export PROJECT_HOME=$HOME/travelhunter
```

### 8.3 변경사항 즉시 적용

```bash
source ~/.bashrc
```

확인:

```bash
echo $APP_ENV
echo $PROJECT_HOME
```

### 8.4 alias 추가

자주 쓰는 명령어를 짧게 만들 수 있다.

```bash
alias ll='ls -la'
alias gs='git status'
```

`.bashrc`에 추가하면 다음 접속부터 계속 사용할 수 있다.

### 실습 7. .bashrc 설정하기

**1단계. .bashrc에 alias 추가**

```bash
echo "alias ll='ls -la'" >> ~/.bashrc
echo "alias myinfo='whoami && pwd'" >> ~/.bashrc
```

**2단계. 변경사항 적용**

```bash
source ~/.bashrc
```

**3단계. 사용해보기**

```bash
ll
myinfo
```

---

## 9. 환경 변수 보안 규칙

### 9.1 해야 할 것

- 비밀번호와 API 키는 코드에 직접 작성하지 않기
- `.env` 파일은 `.gitignore`에 추가하기
- 팀원에게는 `.env.example`만 공유하기
- 서버마다 다른 설정값은 환경 변수로 관리하기

### 9.2 하지 말아야 할 것

```python
DB_PASSWORD = "mySecretPassword123"
```

위처럼 코드에 비밀번호를 직접 작성하면 안 된다. 환경 변수에서 읽어와야 한다.

```python
import os

DB_PASSWORD = os.getenv("DB_PASSWORD")
```

### 9.3 .env를 실수로 git에 올렸다면

**1단계. .gitignore에 .env 추가**

```bash
echo ".env" >> .gitignore
```

**2단계. git 추적에서 제거**

파일은 유지하고 git 추적만 제거한다.

```bash
git rm --cached .env
```

**3단계. 커밋**

```bash
git commit -m "chore: remove env file from git tracking"
```

**4단계. 비밀번호 변경**

이미 GitHub에 올라간 비밀번호나 API 키는 반드시 새로 발급해야 한다.

> **주의**
> Git에서 `.env` 파일을 삭제해도 이전 커밋 기록에는 값이 남아 있을 수 있다. 실수로 올렸다면 비밀번호와 API 키를 즉시 변경한다.

---

## 10. 종합 실습

Python 프로젝트 디렉토리를 만들고, 가상환경, 패키지 설치, 환경 변수, `.env` 파일까지 한 번에 구성한다.

**1단계. 프로젝트 디렉토리 생성**

```bash
mkdir ~/deploy_basic_practice
cd ~/deploy_basic_practice
```

**2단계. 가상환경 생성 및 활성화**

```bash
python3 -m venv venv
source venv/bin/activate
```

**3단계. 패키지 설치**

```bash
pip install requests python-dotenv
```

**4단계. requirements.txt 생성**

```bash
pip freeze > requirements.txt
```

**5단계. .env 생성**

```bash
vim .env
```

내용 입력:

```bash
APP_NAME=travelhunter
APP_ENV=development
API_KEY=test-key-1234
```

**6단계. Python 파일 생성**

```bash
vim app.py
```

내용 입력:

```python
import os
from dotenv import load_dotenv

load_dotenv()

print("APP_NAME:", os.getenv("APP_NAME"))
print("APP_ENV:", os.getenv("APP_ENV"))
print("API_KEY:", os.getenv("API_KEY"))
```

**7단계. 실행**

```bash
python app.py
```

**8단계. .gitignore 생성**

```bash
echo ".env" >> .gitignore
echo "venv/" >> .gitignore
```

**9단계. .env.example 생성**

```bash
vim .env.example
```

내용 입력:

```bash
APP_NAME=
APP_ENV=
API_KEY=
```

**10단계. 결과 파일 확인**

```bash
ls -la
```

확인할 파일:

```bash
app.py
.env
.env.example
.gitignore
requirements.txt
venv/
```

---

## 11. 명령어 요약

| 명령어 | 동작 |
| --- | --- |
| `sudo apt update` | 패키지 목록 최신화 |
| `sudo apt install 패키지명` | 패키지 설치 |
| `sudo apt remove 패키지명` | 패키지 삭제 |
| `systemctl status nginx` | nginx 실행 상태 확인 |
| `sudo systemctl start nginx` | nginx 시작 |
| `sudo systemctl stop nginx` | nginx 중지 |
| `python3 -m venv venv` | Python 가상환경 생성 |
| `source venv/bin/activate` | 가상환경 활성화 |
| `deactivate` | 가상환경 종료 |
| `pip install 패키지명` | Python 패키지 설치 |
| `pip list` | 설치된 Python 패키지 확인 |
| `pip freeze > requirements.txt` | 패키지 목록 저장 |
| `pip install -r requirements.txt` | 패키지 목록 기반 설치 |
| `printenv` | 환경 변수 전체 확인 |
| `echo $변수명` | 특정 환경 변수 확인 |
| `export 변수명=값` | 임시 환경 변수 설정 |
| `source ~/.bashrc` | `.bashrc` 변경사항 적용 |
| `echo ".env" >> .gitignore` | `.env`를 Git 추적에서 제외 |

---

## 과제

1. `~/python_practice` 디렉토리를 만들고 Python 가상환경을 생성한다.
2. 가상환경 안에서 `requests`, `python-dotenv`를 설치한다.
3. `pip freeze > requirements.txt`로 패키지 목록을 저장한다.
4. `.env` 파일에 `APP_ENV=development`와 `DB_PASSWORD=1234`를 작성한다.
5. Python 코드에서 `.env` 값을 읽어 출력한다.
6. `.gitignore`에 `.env`와 `venv/`를 추가한다.
7. `.env.example` 파일을 만들어 필요한 환경 변수 이름만 정리한다.