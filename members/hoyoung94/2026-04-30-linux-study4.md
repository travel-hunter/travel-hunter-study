# Linux Session 4 핵심 요약

## 1. 핵심 흐름

```text
IP 확인 → 라우팅 확인 → 연결 테스트 → 포트 확인 → 웹 서버 확인
```

## 2. IP와 인터페이스 확인

```bash
ip addr
ip link
```

- `ip addr`: 서버 IP 확인
- `ip link`: 네트워크 인터페이스 상태 확인
- `lo`: 자기 자신
- `ens33`, `eth0`: 네트워크 인터페이스

## 3. 라우팅 확인

```bash
ip route
```

- `default`: 외부로 나가는 기본 경로
- `via`: 게이트웨이 주소
- `dev`: 사용하는 인터페이스

기본 라우트가 없으면 외부 인터넷 접속이 안 될 수 있다.

## 4. 연결 테스트

```bash
ping 8.8.8.8
ping google.com
```

- `ping 8.8.8.8`: 인터넷 연결 확인
- `ping google.com`: DNS 동작까지 확인

`ping`이 없으면 설치한다.

```bash
sudo apt install iputils-ping
```

## 5. 포트 확인

```bash
ss -tulpn
ss -tulpn | grep :80
```

- 열린 포트와 사용 중인 프로세스를 확인한다.
- `80`: HTTP
- `443`: HTTPS
- `22`: SSH
- `8000`: 개발 서버에서 자주 사용

## 6. Nginx 확인

```bash
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
```

- `active (running)`: 실행 중
- `inactive (dead)`: 중지됨

접속 테스트:

```bash
curl http://localhost
curl http://서버IP
```

## 7. 자주 나는 문제

### nginx가 꺼져 있는 경우

```bash
sudo systemctl start nginx
```

### 80번 포트가 열려 있지 않은 경우

```bash
ss -tulpn | grep :80
```

### localhost로만 실행한 경우

외부 접속이 안 될 수 있다.

외부 접속을 허용하려면 서버를 `0.0.0.0`으로 실행해야 한다.

예:

```bash
python3 -m http.server 8000 --bind 0.0.0.0
```

## 8. 반드시 기억할 것

- `ip addr`로 IP를 확인한다.
- `ip route`로 기본 경로를 확인한다.
- `ping 8.8.8.8`로 인터넷 연결을 확인한다.
- `ping google.com`으로 DNS를 확인한다.
- `ss -tulpn`으로 열린 포트를 확인한다.
- `curl http://localhost`로 웹 서버 접속을 테스트한다.
- Nginx는 기본적으로 80번 포트를 사용한다.