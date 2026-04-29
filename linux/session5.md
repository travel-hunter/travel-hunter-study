# Session 5. SSH

> 실습 환경: VMware + Ubuntu Server 24.04 + MobaXterm (SSH)
> 

## 학습 목표

- SSH로 원격 서버(EC2)에 접속하고 파일을 주고받을 수 있다
- SSH 단축키 설정으로 접속 명령어를 간소화할 수 있다

---

## 1. SSH — 원격 서버 접속

### 1.1 SSH란

**S**ecure **SH**ell의 약자로, 원격 서버에 안전하게 접속하는 프로토콜이다. EC2 같은 클라우드 서버에 들어갈 때 항상 사용한다.

### 1.2 실습 환경 구성

서버 두 대를 사용해 SSH 키 기반 접속을 실습한다.

- 서버1: `192.168.0.100`
- 서버2: `192.168.0.101`

목표: **서버1 → 서버2로 SSH 키 기반 접속**

### 1.3 두 번째 서버 준비

Session 1에서 만든 Ubuntu Server 가상머신을 그대로 복제해 서버2를 만든다.

**1단계. VMware에서 가상머신 종료**

서버1(기존 가상머신)을 먼저 정상 종료한다.

```bash
sudo poweroff
```

**2단계. 가상머신 복제 (Clone)**

1. VMware Workstation에서 서버1 가상머신을 우클릭 → `Manage` → `Clone`
2. `The current state in the virtual machine` 선택 → Next
3. `Create a full clone` 선택 (Linked clone이 아닌 완전한 복사본)
4. 가상머신 이름: `ubuntu-server-2` (원하는 이름)
5. Finish

**3단계. 두 가상머신 모두 실행**

서버1, 서버2를 동시에 부팅한다.

**4단계. 호스트명 변경 (서버2에서)**

복제한 직후에는 호스트명이 동일해서 헷갈린다. 서버2에서 호스트명을 변경한다.

```bash
sudo hostnamectl set-hostname ubuntu-server-2
```

재로그인하면 프롬프트가 바뀐 것을 확인할 수 있다.

**5단계. 각 서버의 IP 확인**

각 서버에서 IP를 확인하고 메모해둔다.

```bash
ip addr
# 또는
hostname -I
```

> 이 문서의 예시 IP(`192.168.0.100`, `192.168.0.101`)는 임의의 값이다. 본인 환경에서 확인한 실제 IP로 바꿔서 진행한다.
> 

**6단계. 서버 간 통신 확인**

서버1에서 서버2로 ping을 보내본다.

```bash
ping -c 4 192.168.0.101
```

응답이 오면 두 서버가 같은 네트워크에 있는 것이다.

> VMware의 네트워크 설정이 두 가상머신 모두 **NAT** 또는 **Bridged**로 통일되어 있어야 서로 통신이 된다.
> 

---

## 2. SSH 키 생성 (서버1에서 실행)

```bash
ssh-keygen -t rsa -b 4096 -C "ubuntu-server"
```

질문이 나오면 그냥 Enter를 3번 눌러도 무방하다.

### 생성 결과

| 파일 | 설명 |
| --- | --- |
| `~/.ssh/id_rsa` | 개인 키 (절대 공유 금지) |
| `~/.ssh/id_rsa.pub` | 공개 키 (서버2에 등록) |

---

## 3. 공개 키 서버2에 복사

### 방법 1 (권장)

```bash
ssh-copy-id ubuntu@192.168.0.101
```

비밀번호를 입력하면 자동으로 등록된다.

### 방법 2 (수동)

서버1에서 공개 키를 출력해 복사한다.

```bash
cat ~/.ssh/id_rsa.pub
```

서버2에서 키를 등록한다.

```bash
mkdir -p ~/.ssh
vim ~/.ssh/authorized_keys
```

복사한 키를 붙여넣은 뒤 권한을 설정한다.

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## 4. SSH 접속 테스트

```bash
ssh ubuntu@192.168.0.101
```

비밀번호 없이 접속되면 성공이다.

---

## 5. 양방향 통신 설정

서버2에서도 동일한 방식으로 키를 생성한 뒤 서버1에 등록하면 서버2에서 서버1로도 접속할 수 있다.

```bash
# 서버2에서
ssh-keygen -t rsa -b 4096 -C "ubuntu-server-2"
ssh-copy-id ubuntu@192.168.0.100
```

---

## 6. 파일 전송 — scp

### 6.1 SCP란

**S**ecure **C**o**P**y의 약자로, SSH 기반으로 안전하게 파일을 복사하는 도구다.

### 6.2 사용 패턴

**기본 사용**

```bash
# 서버1 → 서버2
scp file.txt ubuntu@192.168.0.101:/home/ubuntu/

# 서버2 → 서버1
scp ubuntu@192.168.0.101:/home/ubuntu/file.txt ./
```

**디렉토리 전송**

```bash
scp -r ./project ubuntu@192.168.0.101:/home/ubuntu/
```

### 6.3 더 빠른 방법 — rsync

`rsync`는 변경된 부분만 전송하므로 대용량·반복 동기화에 유리하다.

```bash
rsync -avz ./project/ ubuntu@192.168.0.101:/home/ubuntu/project/
```

---

## 7. SSH 단축키 설정

매번 긴 명령어를 입력하는 대신, `~/.ssh/config` 파일로 단축 설정을 만들 수 있다.

```bash
vim ~/.ssh/config
```

### 설정 내용

```
Host server2
    HostName 192.168.0.101
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
    Port 22
```

권한 설정:

```bash
chmod 600 ~/.ssh/config
```

### 사용 방법

```bash
# 기존
ssh ubuntu@192.168.0.101

# 단축
ssh server2
```

---

## 9. 명령어 요약

| 명령어 | 동작 |
| --- | --- |
| `ssh-keygen -t rsa` | SSH 키 생성 |
| `ssh-copy-id user@호스트` | 공개 키 자동 등록 |
| `ssh user@호스트` | SSH 접속 |
| `ssh -i 키 user@호스트` | 특정 키로 접속 |
| `scp 파일 user@호스트:경로` | 파일 전송 |
| `scp -r 디렉토리 user@호스트:경로` | 디렉토리 전송 |
| `rsync -avz 원본/ user@호스트:경로/` | 효율적인 동기화 |
| `chmod 600 키파일` | SSH 키 권한 설정 (필수) |

---

## 과제

1. Ubuntu 서버 2대를 준비하고 SSH 키 기반 접속을 구성한다.
2. 서버 간 `scp`로 파일을 주고받는다.
3. `~/.ssh/config`로 접속 단축 명령어를 만들어본다.