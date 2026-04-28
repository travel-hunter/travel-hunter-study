# Session 3. 프로세스·서비스 관리

> 실습 환경: VMware + Ubuntu Server 24.04 + MobaXterm (SSH)

## 학습 목표

- 실행 중인 프로세스를 확인할 수 있다
- PID를 이용해 프로세스를 종료할 수 있다
- 백그라운드 실행과 서비스 실행의 차이를 이해한다
- `systemd`로 앱을 서비스로 등록할 수 있다
- `journalctl`로 앱 로그를 확인할 수 있다
- CPU, 메모리, 디스크 사용량을 확인할 수 있다

---

## 1. 프로세스란

프로세스는 **실행 중인 프로그램**을 의미한다.

다음 명령어를 각각 실행하면 프로세스가 생성된다. 터미널을 하나 더 열어 `nginx` 명령어를 입력해본다.

```bash
sleep 1000
nginx
```

앱 개발·배포 과정에서 프로세스 관리는 핵심이다.

| 상황 | 필요한 작업 |
| --- | --- |
| 앱이 실행 중인지 확인 | 프로세스 확인 |
| 앱이 멈췄는지 확인 | 프로세스 상태 확인 |
| 포트가 이미 사용 중일 때 | 어떤 프로세스가 점유 중인지 확인 |
| 앱을 재시작해야 할 때 | 프로세스 종료 또는 서비스 재시작 |

각 프로세스는 고유 번호를 가지며, 이를 **PID(Process ID)**라고 한다.

---

## 2. 프로세스 확인

### 2.1 전체 프로세스 보기

```bash
ps -ef
```

현재 시스템에서 실행 중인 모든 프로세스 목록을 출력한다.

### 2.2 일부만 보기

```bash
ps -ef | head -10
```

전체 결과 중 앞 10줄만 출력한다.

### 2.3 특정 프로세스 찾기

```bash
ps -ef | grep nginx
```

실행 중인 nginx 프로세스만 필터링해서 보여준다.

> - `|` (파이프): 앞 명령어의 결과를 뒤 명령어로 전달
> - `grep`: 특정 문자열이 포함된 줄만 추출

### 실습 1. 프로세스 확인

**1단계. 전체 프로세스 확인**

```bash
ps -ef | head -20
```

**2단계. nginx 프로세스 찾기**

```bash
ps -ef | grep nginx
```

**3단계. 현재 사용자 확인**

```bash
whoami
```

---

## 3. CPU 부하 만들기 (핵심 실습)

### 3.1 CPU 부하 생성

```bash
yes > /dev/null &
yes > /dev/null &
```

각 부분의 의미:

- `yes`: 문자열을 무한 반복 출력
- `/dev/null`: 출력을 버리는 장치 (Linux의 휴지통)
- `>`: 출력 방향 변경
- `&`: 백그라운드 실행

결과적으로 CPU를 지속적으로 사용하는 프로세스가 생성된다.

### 3.2 실행 확인

```bash
ps -ef | grep yes
```

실행 중인 `yes` 프로세스를 확인한다.

### 실습 2. 프로세스 실행 후 종료하기

**1단계. 5분 동안 실행되는 프로세스 만들기**

```bash
sleep 300 &
```

**2단계. sleep 프로세스 찾기**

```bash
ps -ef | grep sleep
```

**3단계. PID 확인 후 종료**

```bash
kill PID
```

예시:

```bash
kill 1234
```

**4단계. 종료 확인**

```bash
ps -ef | grep sleep
```

---

## 4. 시스템 상태 확인

### 4.1 top (기본 도구)

```bash
top
```

실시간으로 시스템 상태를 확인한다.

확인할 항목:

- CPU 사용량이 높은 프로세스
- 각 프로세스의 PID

종료: `q`

### 4.2 htop (선택)

```bash
sudo apt install htop -y
htop
```

`top`보다 시각적으로 보기 쉬운 인터페이스를 제공한다.

종료: `q`

### 실습 3. 백그라운드 실행 & 서비스 테스트 (nginx)

**1단계. nginx 실행**

```bash
sudo systemctl start nginx
```

웹 서버를 백그라운드에서 실행한다.

**2단계. 프로세스 확인**

```bash
ps -ef | grep nginx
```

확인할 것:

- master process
- worker process

**3단계. 포트 확인**

```bash
ss -tulpn | grep 80
```

nginx가 80번 포트를 사용 중인지 확인한다.

**4단계. 접속 확인**

```bash
curl http://localhost
```

nginx 기본 페이지의 응답을 확인한다.

**5단계. 서비스 중지**

```bash
sudo systemctl stop nginx
```

**6단계. 종료 확인**

```bash
ps -ef | grep nginx
```

nginx 프로세스가 사라졌는지 확인한다.

**7단계. 포트 해제 확인**

```bash
ss -tulpn | grep 80
```

아무 결과도 출력되지 않으면 80번 포트 해제가 완료된 것이다.

---

## 5. 프로세스 분석

### 5.1 특정 PID 상세 보기

```bash
ps -fp PID
```

해당 PID의 상세 정보(명령어, 실행 시간 등)를 확인한다.

### 5.2 프로세스가 사용하는 자원 확인

```bash
sudo lsof -p PID
```

해당 프로세스가 열고 있는 파일과 네트워크를 확인한다.

### 실습 4. systemd 서비스 등록하기

이 실습에서는 Python 내장 웹 서버를 systemd 서비스로 등록한다. 실제 FastAPI 앱도 동일한 방식으로 등록할 수 있다.

**1단계. 앱 디렉토리 생성**

```bash
mkdir -p ~/service_test
cd ~/service_test
```

**2단계. 테스트 파일 생성**

```bash
echo "TravelHunter service test" > index.html
```

**3단계. 현재 사용자 확인**

```bash
whoami
```

일반적인 출력:

```bash
ubuntu
```

**4단계. 서비스 파일 생성**

```bash
sudo vim /etc/systemd/system/traveltest.service
```

내용 입력:

```ini
[Unit]
Description=TravelHunter Test Web Service
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/service_test
ExecStart=/usr/bin/python3 -m http.server 8000
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

> **주의**
> `User=ubuntu`와 `/home/ubuntu/service_test`는 본인 환경에 맞춰야 한다.
> `whoami` 결과가 `ubuntu`가 아니라면 사용자명을 변경한다.

**5단계. systemd 설정 다시 읽기**

```bash
sudo systemctl daemon-reload
```

**6단계. 서비스 시작**

```bash
sudo systemctl start traveltest
```

**7단계. 서비스 상태 확인**

```bash
sudo systemctl status traveltest
```

다음 메시지가 보이면 정상이다.

```bash
active (running)
```

**8단계. 접속 확인**

```bash
curl http://localhost:8000
```

출력 예시:

```bash
TravelHunter service test
```

**9단계. 자동 시작 등록**

```bash
sudo systemctl enable traveltest
```

**10단계. 로그 확인**

```bash
sudo journalctl -u traveltest -n 50
```

실시간 로그 확인:

```bash
sudo journalctl -u traveltest -f
```

종료는 `Ctrl + C`.

### 실습 5. 서비스 제어하기

```bash
# 서비스 중지
sudo systemctl stop traveltest

# 서비스 시작
sudo systemctl start traveltest

# 서비스 재시작
sudo systemctl restart traveltest

# 서비스 상태 확인
sudo systemctl status traveltest

# 자동 시작 해제
sudo systemctl disable traveltest
```

---

## 6. 프로세스 종료

### 6.1 정상 종료

```bash
kill PID
```

프로세스에 정상 종료를 요청한다 (SIGTERM).

### 6.2 강제 종료

```bash
kill -9 PID
```

즉시 종료한다 (SIGKILL).

> 강제 종료는 꼭 필요할 때만 사용한다. 정상 종료가 우선이다.

### 실습 6. 문제 해결 흐름

**1단계. CPU 부하 생성**

```bash
yes > /dev/null &
yes > /dev/null &
```

**2단계. top 실행**

```bash
top
```

CPU 사용량이 높은 PID를 확인한다.

**3단계. 상세 확인**

```bash
ps -fp PID
```

어떤 프로세스인지 확인한다.

**4단계. 종료**

```bash
kill PID
```

**5단계. 종료되지 않을 경우**

```bash
kill -9 PID
```

**6단계. 정상 확인**

```bash
top
```

CPU 사용률이 내려가면 성공이다.

---

## 7. 백그라운드 실행

```bash
sleep 300 &
```

300초 동안 실행되는 프로세스를 백그라운드로 실행한다.

확인:

```bash
ps -ef | grep sleep
```

종료:

```bash
kill PID
```

---

## 8. 서비스 관리 (systemd)

```bash
# 서비스 상태 확인
sudo systemctl status nginx

# 시작 / 종료
sudo systemctl start nginx
sudo systemctl stop nginx

# 재시작
sudo systemctl restart nginx
```

---

## 9. 명령어 요약

| 명령어 | 동작 |
| --- | --- |
| `ps -ef` | 전체 프로세스 보기 |
| `ps -ef \| grep 이름` | 특정 프로세스 찾기 |
| `top` / `htop` | 실시간 리소스 모니터링 |
| `ps -fp PID` | 프로세스 정보 보기 |
| `lsof -p PID` | 열고 있는 파일·포트 확인 |
| `kill PID` | 정상 종료 |
| `kill -9 PID` | 강제 종료 |
| `명령어 &` | 백그라운드 실행 |
| `yes > /dev/null &` | CPU 부하 테스트 |
| `systemctl status nginx` | 서비스 상태 확인 |
| `systemctl start / stop / restart` | 서비스 관리 |

---

## 과제

1. `top` 명령어를 실행해서 CPU 사용량이 가장 높은 프로세스 3개를 확인하고, PID와 함께 정리한다.

2. `yes > /dev/null &` 명령어를 2번 실행하여 CPU 부하를 만든 뒤, `top`과 `ps -fp PID`로 해당 프로세스를 확인하고 `kill`로 종료한다.

   다음 항목을 정리한다:
   - 생성된 프로세스 PID
   - CPU 사용량 확인 결과
   - 종료 전 / 종료 후 변화

3. `sleep 300 &` 명령어로 백그라운드 프로세스를 실행한 뒤, `ps -ef`로 확인하고 `kill`로 종료한다.

   다음 항목을 정리한다:
   - 실행된 PID
   - 백그라운드 실행 상태 확인 방법
   - 종료 결과