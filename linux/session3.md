# Session 3. 프로세스·서비스 관리

> 실습 환경: Ubuntu Server (단일 서버)


## 이번 세션 목표

-   실행 중인 프로세스를 확인할 수 있다.
-   CPU 사용량이 높은 프로세스를 찾을 수 있다.
-   PID를 이용해 프로세스를 종료할 수 있다.
-   백그라운드 실행을 이해할 수 있다.
-   systemd로 서비스를 관리할 수 있다.

---

## 1. 프로세스란

프로세스는 실행 중인 프로그램이다.

``` bash
sleep 1000
nginx
```

프로세스는 고유한 번호(PID)를 가진다.

---

## 2. 프로세스 확인

### 전체 프로세스 보기

``` bash
ps -ef
```

### 일부만 보기

``` bash
ps -ef | head -10
```

### 특정 프로세스 찾기

``` bash
ps -ef | grep nginx
```

---

## 실습 1. 프로세스 확인

``` bash
ps -ef | head -20
ps -ef | grep nginx
whoami
```

---

## 3. CPU 부하 생성

``` bash
yes > /dev/null &
yes > /dev/null &
```

---

## 4. 시스템 상태 확인

``` bash
top
```

종료: q

---

## 5. 프로세스 분석

``` bash
ps -fp PID
sudo lsof -p PID
```

---

## 6. 프로세스 종료

``` bash
kill PID
kill -9 PID
```

---

## 7. 백그라운드 실행

``` bash
sleep 300 &
```

---

## 8. 서비스 관리

``` bash
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

---

## 실습 3. nginx 서비스 테스트

``` bash
sudo systemctl start nginx
ps -ef | grep nginx
ss -tulpn | grep 80
curl http://localhost
sudo systemctl stop nginx
```

---

## 요약

  명령어      설명
  ----------- ---------------
  ps -ef      전체 프로세스
  top         실시간 상태
  kill        종료
  systemctl   서비스 관리

---

## 과제

1.  top 실행 후 CPU 높은 프로세스 3개 확인
2.  yes 실행 후 PID 확인 및 종료
3.  sleep 실행 후 종료
4.  nginx stop/start 확인
