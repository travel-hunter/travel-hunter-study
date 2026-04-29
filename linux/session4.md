# Session 4. 네트워킹 기초

> 실습 환경: VMware + Ubuntu Server 24.04 + MobaXterm (SSH)

## 학습 목표

- 서버의 IP 주소를 확인할 수 있다
- IPv4와 IPv6 주소를 구분할 수 있다
- 네트워크 인터페이스 상태를 확인할 수 있다
- `ping`으로 서버 연결 상태를 확인할 수 있다
- `traceroute`로 네트워크 경로를 확인할 수 있다
- `netstat`, `ss`로 열린 포트를 확인할 수 있다
- Nginx 웹 서버를 실행하고 포트 상태를 확인할 수 있다
- 앱 개발·배포 중 발생하는 네트워크 문제를 기본적으로 점검할 수 있다

---

## 1. 앱 배포에서 네트워크가 필요한 이유

웹 앱을 개발하거나 배포할 때는 항상 네트워크를 다루게 된다. 백엔드 서버나 웹 서버를 실행하면 다음과 같은 주소로 접속한다.

```
http://localhost
http://127.0.0.1
http://서버IP
http://서버IP:포트번호
```

이때 다루는 핵심 요소는 다음과 같다.

| 요소 | 의미 |
| --- | --- |
| IP 주소 | 서버의 위치 |
| 포트 | 서버 안에서 실행 중인 서비스 번호 |
| 프로토콜 | HTTP, HTTPS, SSH 등 통신 방식 |
| 네트워크 인터페이스 | 서버가 네트워크에 연결되는 장치 |
| 방화벽 / 보안그룹 | 외부 접속 허용 여부 |

---

## 2. IP 주소 확인

### 2.1 IP 주소란

IP 주소는 서버나 PC를 네트워크에서 식별하기 위한 주소다. 앱 배포 과정에서 자주 마주치는 IP는 다음과 같다.

| IP | 의미 |
| --- | --- |
| `127.0.0.1` | 내 컴퓨터 자신 |
| `localhost` | 내 컴퓨터 자신 |
| `192.168.x.x` | 내부 네트워크 IP |
| `10.x.x.x` | 내부 네트워크 IP |
| 공인 IP | 인터넷에서 접속 가능한 서버 IP |

### 2.2 IPv4

IPv4는 가장 널리 사용되는 IP 형식으로, 숫자 4묶음으로 구성된다.

```
192.168.0.10
13.125.100.20
127.0.0.1
```

범위:

```
0.0.0.0 ~ 255.255.255.255
```

### 2.3 IPv6

IPv6는 더 긴 주소 형식이다.

```
2001:db8::1
fe80::a00:27ff:fe4e:66a1
```

실습에서는 IPv4를 주로 사용하지만, 서버에서 IPv6 주소도 함께 확인되는 경우가 있다.

### 실습 1. 내 서버 IP 주소 확인

**1단계. IP 주소 확인**

```bash
ip addr
```

또는 짧게:

```bash
ip a
```

**2단계. IPv4 주소 찾기**

출력에서 `inet` 항목을 찾는다.

예시:

```
inet 192.168.0.10/24
inet 127.0.0.1/8
```

| 항목 | 의미 |
| --- | --- |
| `127.0.0.1` | 내 컴퓨터 자신 |
| `192.168.0.10` | 현재 서버의 내부 IP |
| `/24` | 네트워크 범위 |

**3단계. IPv6 주소 찾기**

출력에서 `inet6` 항목을 찾는다.

예시:

```
inet6 fe80::a00:27ff:fe4e:66a1/64
```

---

## 3. 네트워크 인터페이스 확인

### 3.1 네트워크 인터페이스란

네트워크 인터페이스는 서버가 네트워크에 연결되는 장치다. Linux에서는 다음과 같은 이름으로 표시된다.

| 이름 | 의미 |
| --- | --- |
| `lo` | 자기 자신에게 접속하는 인터페이스 (loopback) |
| `eth0` | 유선 네트워크 인터페이스 |
| `ens33` | 가상머신에서 자주 보이는 인터페이스 |
| `wlan0` | 무선 네트워크 인터페이스 |

### 실습 2. 네트워크 인터페이스 상태 확인

**1단계. 인터페이스 목록 확인**

```bash
ip link
```

출력 예시:

```
1: lo: <LOOPBACK,UP,LOWER_UP>
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

| 상태 | 의미 |
| --- | --- |
| `UP` | 인터페이스가 켜져 있음 |
| `DOWN` | 인터페이스가 꺼져 있음 |
| `LOOPBACK` | 자기 자신용 인터페이스 |
| `BROADCAST` | 일반 네트워크 인터페이스 |

**2단계. ifconfig로 확인**

```bash
ifconfig
```

명령어가 없다면 설치한다.

Ubuntu:

```bash
sudo apt update
sudo apt install net-tools
```

Amazon Linux / CentOS:

```bash
sudo yum install net-tools
```

---

## 4. 라우팅 확인

### 4.1 라우팅이란

라우팅은 서버가 외부 네트워크로 나가는 경로를 결정하는 과정이다. 앱 서버가 외부 API, GitHub, 패키지 저장소 등에 접근하려면 라우팅이 정상이어야 한다.

### 실습 3. 기본 게이트웨이 확인

```bash
ip route
```

출력 예시:

```
default via 192.168.0.1 dev eth0
192.168.0.0/24 dev eth0 proto kernel scope link src 192.168.0.10
```

| 항목 | 의미 |
| --- | --- |
| `default` | 기본 경로 |
| `via 192.168.0.1` | 게이트웨이 주소 |
| `dev eth0` | 사용하는 네트워크 인터페이스 |
| `src 192.168.0.10` | 내 서버 IP |

---

## 5. ping으로 연결 확인

### 5.1 ping이란

`ping`은 특정 서버와의 연결 상태를 확인하는 명령어다. 앱 배포 중 다음과 같은 상황에서 자주 사용한다.

| 상황 | 확인 대상 |
| --- | --- |
| 인터넷 연결 확인 | `8.8.8.8` |
| 도메인 연결 확인 | `google.com` |
| 서버 내부 확인 | `127.0.0.1` |
| 다른 서버 연결 확인 | EC2 퍼블릭 IP |

### 실습 4. ping 테스트

**1단계. 내 컴퓨터 자신 확인**

```bash
ping 127.0.0.1
```

**2단계. 외부 IP 연결 확인**

```bash
ping 8.8.8.8
```

**3단계. 도메인 연결 확인**

```bash
ping google.com
```

**4단계. 결과 확인**

성공 예시:

```
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=32.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=31.8 ms
```

| 항목 | 의미 |
| --- | --- |
| `icmp_seq` | 몇 번째 요청인지 |
| `ttl` | 패킷 생존 시간 |
| `time` | 응답 시간 |

---

## 6. traceroute로 경로 확인

### 6.1 traceroute란

`traceroute`는 내 서버에서 목적지 서버까지 어떤 경로로 이동하는지 확인하는 명령어다. 서버가 특정 API나 사이트에 접속이 느리거나 안 될 때 활용할 수 있다.

### 실습 5. traceroute 실행

**1단계. traceroute 설치**

Ubuntu:

```bash
sudo apt update
sudo apt install traceroute
```

Amazon Linux / CentOS:

```bash
sudo yum install traceroute
```

**2단계. Google까지 경로 확인**

```bash
traceroute google.com
```

**3단계. GitHub까지 경로 확인**

```bash
traceroute github.com
```

출력 예시:

```
1  192.168.0.1
2  10.10.0.1
3  100.64.0.1
4  142.250.xxx.xxx
```

각 줄은 목적지까지 가는 중간 네트워크 장비를 의미한다.

---

## 7. 열린 포트 확인

### 7.1 포트란

포트는 서버 안에서 실행 중인 서비스의 입구 번호다. 앱 개발·배포에서는 다음 포트를 자주 사용한다.

| 포트 | 서비스 | 용도 |
| --- | --- | --- |
| 22 | SSH | 서버 원격 접속 |
| 80 | HTTP | 웹 서비스 |
| 443 | HTTPS | 보안 웹 서비스 |
| 3000 | React / Next.js | 프론트엔드 개발 서버 |
| 8000 | FastAPI / Django | 백엔드 API 서버 |
| 5432 | PostgreSQL | 데이터베이스 |
| 6379 | Redis | 캐시 서버 |

### 실습 6. netstat으로 열린 포트 확인

**1단계. 열린 포트 확인**

```bash
netstat -tulpn
```

옵션 의미:

| 옵션 | 의미 |
| --- | --- |
| `-t` | TCP |
| `-u` | UDP |
| `-l` | LISTEN 상태 |
| `-p` | 프로세스 정보 |
| `-n` | 숫자로 표시 |

**2단계. 특정 포트만 확인**

```bash
netstat -tulpn | grep :80
```

### 실습 7. ss로 열린 포트 확인

`ss`는 최신 Linux에서 권장되는 포트 확인 명령어다.

```bash
ss -tulpn
```

특정 포트만 확인:

```bash
ss -tulpn | grep :80
```

출력 예시:

```
tcp LISTEN 0 511 0.0.0.0:80 0.0.0.0:* users:(("nginx",pid=1234))
```

| 항목 | 의미 |
| --- | --- |
| `LISTEN` | 접속 대기 중 |
| `0.0.0.0:80` | 모든 IP에서 80번 포트 접속 가능 |
| `127.0.0.1:80` | 내 컴퓨터에서만 접속 가능 |
| `pid=1234` | 해당 포트를 사용하는 프로세스 ID |

---

## 8. Nginx 웹 서버 실행 후 네트워크 확인

앱 서버가 실행되면 실제로 포트가 열렸는지 확인해야 한다. Nginx는 실제 배포 환경에서도 널리 쓰이는 웹 서버이며, HTTP 웹 서비스는 기본적으로 80번 포트를 사용한다.

### 실습 8. Nginx 웹 서버 실행

**1단계. Nginx 설치**

```bash
sudo apt update
sudo apt install nginx
```

**2단계. Nginx 실행 상태 확인**

```bash
sudo systemctl status nginx
```

출력에서 `active (running)`이 보이면 정상이다.

**3단계. 열린 포트 확인**

```bash
ss -tulpn | grep :80
```

또는:

```bash
netstat -tulpn | grep :80
```

출력 예시:

```
tcp LISTEN 0 511 0.0.0.0:80 0.0.0.0:* users:(("nginx",pid=1234))
```

| 항목 | 의미 |
| --- | --- |
| `LISTEN` | 접속 대기 중 |
| `0.0.0.0:80` | 모든 IP에서 80번 포트 접속 가능 |
| `nginx` | 포트를 사용하는 프로그램 |

**4단계. curl로 접속 확인**

```bash
curl http://localhost
```

또는:

```bash
curl http://127.0.0.1
```

Nginx 기본 HTML 내용이 출력되면 정상이다.

**5단계. 내 서버 IP로 접속 확인**

먼저 서버 IP를 확인한다.

```bash
ip addr
```

그다음 curl로 접속한다.

```bash
curl http://서버IP
```

예시:

```bash
curl http://192.168.0.10
```

VMware 환경에서는 Windows 호스트 PC의 브라우저에서 다음 주소로 접속해볼 수도 있다.

```
http://서버IP
```

**6단계. Nginx 중지**

```bash
sudo systemctl stop nginx
```

**7단계. 포트가 닫혔는지 확인**

```bash
ss -tulpn | grep :80
```

아무 결과도 출력되지 않으면 80번 포트가 닫힌 것이다.

**8단계. Nginx 다시 실행**

```bash
sudo systemctl start nginx
```

---

## 9. 앱 배포 중 자주 만나는 네트워크 문제

### 9.1 서버는 실행했는데 접속이 안 되는 경우

확인 순서:

```bash
# 1. 서버 IP 확인
ip addr

# 2. 서버가 포트를 열었는지 확인
ss -tulpn

# 3. 특정 포트 확인
ss -tulpn | grep :80

# 4. 로컬에서 접속 확인
curl http://localhost

# 5. 외부 접속용 주소로 실행되었는지 확인
```

### 9.2 localhost로만 실행한 경우

서버가 다음처럼 실행되면 서버 내부에서만 접속 가능하다.

```
127.0.0.1:8000
```

외부에서 접속 가능하게 하려면 다음처럼 실행한다.

```
0.0.0.0:8000
```

FastAPI 예시:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

Django 예시:

```bash
python manage.py runserver 0.0.0.0:8000
```

React / Vite 예시:

```bash
npm run dev -- --host 0.0.0.0
```

### 9.3 포트가 이미 사용 중인 경우

에러 예시:

```
Address already in use
```

확인:

```bash
ss -tulpn | grep :80
```

또는:

```bash
netstat -tulpn | grep :80
```

프로세스 종료:

```bash
kill {PID}
```

강제 종료:

```bash
kill -9 {PID}
```

> 실습 환경에서 임의로 프로세스를 종료하기 전에, 어떤 프로그램인지 먼저 확인하는 것이 좋다.

---

## 10. 종합 실습

Nginx 웹 서버를 실행하고, IP와 포트 상태를 확인한 뒤 접속 테스트까지 진행한다.

**1단계. 내 IP 확인**

```bash
ip addr
```

**2단계. 라우팅 확인**

```bash
ip route
```

**3단계. 인터넷 연결 확인**

```bash
ping -c 4 8.8.8.8
```

**4단계. 도메인 연결 확인**

```bash
ping -c 4 google.com
```

**5단계. Nginx 설치 및 실행 확인**

```bash
sudo apt update
sudo apt install nginx
sudo systemctl status nginx
```

**6단계. 80번 포트 확인**

```bash
ss -tulpn | grep :80
```

**7단계. curl로 로컬 접속 확인**

```bash
curl http://localhost
```

**8단계. 서버 IP로 접속 확인**

```bash
curl http://서버IP
```

예시:

```bash
curl http://192.168.0.10
```

**9단계. Nginx 중지 후 포트 확인**

```bash
sudo systemctl stop nginx
ss -tulpn | grep :80
```

**10단계. Nginx 다시 실행**

```bash
sudo systemctl start nginx
```

---

## 11. 명령어 요약

| 명령어 | 동작 |
| --- | --- |
| `ip addr` | IP 주소 확인 |
| `ip a` | IP 주소 확인 (짧은 형태) |
| `ip link` | 네트워크 인터페이스 확인 |
| `ip route` | 라우팅 테이블 확인 |
| `ifconfig` | 네트워크 인터페이스 정보 확인 |
| `ping -c 4 8.8.8.8` | 인터넷 연결 확인 |
| `ping -c 4 google.com` | 도메인 연결 확인 |
| `traceroute google.com` | 목적지까지 네트워크 경로 확인 |
| `netstat -tulpn` | 열린 포트 확인 |
| `ss -tulpn` | 열린 포트 확인 (최신 방식) |
| `ss -tulpn \| grep :80` | 80번 포트 확인 |
| `curl http://localhost` | 로컬 웹 서버 접속 테스트 |
| `sudo systemctl status nginx` | Nginx 실행 상태 확인 |
| `sudo systemctl stop nginx` | Nginx 중지 |
| `sudo systemctl start nginx` | Nginx 실행 |

---

## 과제

1. `ip addr` 명령어로 본인 Linux 환경의 IP 주소를 확인한다.
2. `ip route` 명령어로 기본 게이트웨이를 확인한다.
3. `ping -c 4 8.8.8.8` 명령어로 인터넷 연결을 확인한다.
4. `ping -c 4 google.com` 명령어로 DNS 연결을 확인한다.
5. Nginx를 실행한 뒤 `ss -tulpn | grep :80`과 `curl http://localhost`로 웹 서버 접속 상태를 확인한다.