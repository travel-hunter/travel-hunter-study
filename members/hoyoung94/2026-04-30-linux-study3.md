# Linux Session 3 핵심 요약

## 핵심 흐름

```text
프로세스 확인 → PID 확인 → 종료 → 서비스 등록 → 로그 확인
```

## 프로세스 확인

```bash
ps -ef
ps -ef | grep 이름
ps -fp PID
top
```

- 프로세스: 실행 중인 프로그램
- PID: 프로세스를 구분하는 숫자 ID
- `PID`는 실제 숫자로 바꿔 입력한다.

예:

```bash
ps -fp 1234
```

## 프로세스 종료

```bash
kill PID
kill -9 PID
```

- `kill PID`: 정상 종료
- `kill -9 PID`: 강제 종료
- 먼저 `kill`을 사용하고, 안 될 때만 `kill -9`를 사용한다.

## 백그라운드 실행

```bash
명령어 &
```

예:

```bash
yes > /dev/null &
sleep 300 &
```

- `&`: 백그라운드 실행
- `yes > /dev/null &`: CPU 부하 테스트

## systemd 서비스 관리

서비스 파일 위치:

```bash
/etc/systemd/system/서비스명.service
```

주요 명령어:

```bash
sudo systemctl daemon-reload
sudo systemctl start 서비스명
sudo systemctl status 서비스명
sudo systemctl stop 서비스명
sudo systemctl restart 서비스명
sudo systemctl enable 서비스명
sudo systemctl disable 서비스명
```

- 서비스 파일 수정 후에는 `daemon-reload` 실행
- 부팅 시 자동 실행은 `enable`

## 로그 확인

```bash
sudo journalctl -u 서비스명 -n 50
sudo journalctl -u 서비스명 -f
```

- `-u`: 특정 서비스 로그
- `-n 50`: 최근 50줄
- `-f`: 실시간 로그

## 반드시 기억할 것

- PID는 실제 숫자로 입력한다.
- 프로세스 종료는 `kill` 후 필요 시 `kill -9`.
- 백그라운드 실행은 `&`.
- 계속 실행할 프로그램은 `systemd` 서비스로 등록한다.
- 서비스 로그는 `journalctl -u 서비스명`으로 확인한다.