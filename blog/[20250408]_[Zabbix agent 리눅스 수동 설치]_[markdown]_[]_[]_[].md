---
# 📦 Zabbix Agent 수동 설치 메뉴얼 (폐쇄망 환경용)

Zabbix Agent를 폐쇄망 서버에 수동으로 설치하고 설정하는 전체 절차입니다.  
**지원 OS:**
- CentOS 7
- CentOS 8 / Rocky Linux 8
- Debian 11

---

## 📁 1. 설치 파일 준비 (공통)

인터넷이 되는 환경에서 아래 경로에서 패키지 다운로드:
- [Zabbix 공식 저장소](https://repo.zabbix.com/zabbix/6.0/)
- OS/버전별 `.rpm` 또는 `.deb` 파일 다운로드

폐쇄망 서버로 파일을 `scp` 또는 USB 등으로 복사

---

## 🛠 2. 설치 방법 (OS 별)

### ✅ CentOS 7

```bash
# 종속 패키지 설치
sudo yum localinstall pcre2-10.23-2.el7.x86_64.rpm

# Zabbix Agent 설치
sudo rpm -ivh zabbix-agent-6.0.4-1.el7.x86_64.rpm
```

### ✅ CentOS 8 / Rocky Linux 8

```bash
# DNF를 사용하는 경우 (로컬 설치)
sudo dnf install zabbix-agent-6.0.4-1.el8.x86_64.rpm pcre2-10.32-2.el8.x86_64.rpm --disablerepo=*

# 또는 rpm 사용
sudo rpm -ivh zabbix-agent-6.0.4-1.el8.x86_64.rpm
```

> **Note:** CentOS 8부터는 `dnf`를 기본 패키지 매니저로 사용합니다.

### ✅ Debian 11

```bash
# Zabbix Agent 설치
sudo dpkg -i zabbix-agent_6.0.4-1+debian11_amd64.deb

# 의존성 자동 설치 (선택)
sudo apt --fix-broken install
```

---

## 🖥 3. 호스트 이름 확인

```bash
hostname
```

---

## ⚙ 4. 설정 파일 수정 (공통)

```bash
sudo vi /etc/zabbix/zabbix_agentd.conf
```

수정할 항목:

```conf
Server=ZABBIX_SERVER_IP
ServerActive=ZABBIX_SERVER_IP
Hostname=해당_호스트_이름
```

---

## 🔁 5. 서비스 시작 및 부팅 시 자동 실행

```bash
sudo systemctl start zabbix-agent
sudo systemctl enable zabbix-agent
```

상태 확인:

```bash
sudo systemctl status zabbix-agent
```

---

## 🔥 6. 방화벽 설정 (RHEL 계열)

```bash
sudo firewall-cmd --add-port=10050/tcp --permanent
sudo firewall-cmd --reload
```

---

## 📌 7. 로그 확인 (공통)

```bash
sudo tail -f /var/log/zabbix/zabbix_agentd.log
```

