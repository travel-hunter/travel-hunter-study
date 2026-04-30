# VMware Ubuntu Clone IP 중복 원인

## 원인

VMware에서 Ubuntu Server를 클론하면 `/etc/machine-id`가 원본과 동일하게 복사될 수 있다.

일부 DHCP 환경에서는 이 `machine-id`를 DHCP Client ID처럼 사용하기 때문에, MAC 주소가 달라도 같은 장비로 인식되어 같은 IP를 받을 수 있다.

## 확인 명령어

```bash
cat /etc/machine-id

```

192.168.100.138
192.168.100.139

# Linux Session 5 - SSH Config 실습 정리

## 1. 실습 목표

긴 SSH 접속 명령어 대신 짧은 별칭으로 서버에 접속한다.

기존 방식:

```bash
ssh ubuntu@192.168.100.139
```

SSH config 사용 후:

```bash
ssh server2
```

---

## 2. SSH config 파일 위치

사용자별 SSH 설정 파일은 아래 경로에 만든다.

```bash
~/.ssh/config
```

파일 생성 또는 수정:

```bash
vim ~/.ssh/config
```

---

## 3. SSH config 예시

```text
Host server2
    HostName 192.168.100.139
    User ubuntu
    Port 22
```

항목 의미:

| 항목 | 의미 |
|---|---|
| `Host server2` | SSH 접속 별칭 |
| `HostName` | 실제 서버 IP 또는 도메인 |
| `User` | 접속할 사용자 |
| `Port` | SSH 포트 번호 |

---

## 4. 권한 설정

SSH config 파일은 권한이 너무 열려 있으면 문제가 될 수 있다.

```bash
chmod 600 ~/.ssh/config
```

권한 의미:

| 권한 | 의미 |
|---|---|
| `600` | 소유자만 읽기/쓰기 가능 |
| 그룹/기타 사용자 | 접근 불가 |

확인:

```bash
ls -l ~/.ssh/config
```

정상 예시:

```text
-rw------- 1 ubuntu ubuntu ... /home/ubuntu/.ssh/config
```

---

## 5. 접속 테스트

```bash
ssh server2
```

정상 접속되면 아래처럼 원격 서버 프롬프트가 표시된다.

```text
ubuntu@ubuntu-server-2:~$
```

현재 화면처럼 `Welcome to Ubuntu ...` 메시지가 나오고 프롬프트가 `ubuntu@ubuntu-server-2`로 바뀌었다면 접속 성공이다.

---

## 6. 화면에 나온 메시지 해석

```text
This system has been minimized by removing packages and content...
```

이 메시지는 오류가 아니다.

Ubuntu Server가 최소 설치 상태라는 안내 메시지이다.  
SSH 접속 문제와는 관련 없다.

---

## 7. 접속 확인 명령어

서버에 제대로 접속했는지 확인한다.

```bash
hostname
whoami
ip addr
```

예상 결과:

```text
ubuntu-server-2
ubuntu
```

---

## 8. 자주 발생하는 문제

### config 파일 권한 문제

증상:

```text
Bad owner or permissions
```

해결:

```bash
chmod 600 ~/.ssh/config
```

### HostName 오타

증상:

```text
Could not resolve hostname
```

해결:

```bash
vim ~/.ssh/config
```

`HostName` 값이 실제 서버 IP와 맞는지 확인한다.

### 접속 거부

증상:

```text
Connection refused
```

확인할 것:

- 서버가 켜져 있는지
- SSH 서비스가 실행 중인지
- 포트 번호가 맞는지

서버에서 확인:

```bash
sudo systemctl status ssh
```

---

## 9. 핵심 정리

- `~/.ssh/config`는 SSH 접속 정보를 저장하는 설정 파일이다.
- `Host`를 사용하면 긴 접속 명령어를 짧은 별칭으로 줄일 수 있다.
- `chmod 600 ~/.ssh/config`로 권한을 제한해야 한다.
- `ssh server2`로 접속했을 때 원격 서버 프롬프트가 나오면 성공이다.
- `Welcome to Ubuntu ...` 메시지는 정상 로그인 메시지이다.
- 최소 설치 안내 메시지는 오류가 아니다.