# 폐쇄망에서 Zabbix Agent 설치 및 설정 가이드

Zabbix Agent를 설치하고 설정하는 과정을 명령어별로 정리했습니다.

## 1. **`yum localinstall`로 의존성 패키지 설치**
```bash
yum localinstall pcre2-10.23-2.el7.x86_64.rpm
```
- **설명**: 
  - 폐쇄망 환경에서는 필요한 패키지를 외부에서 미리 다운로드한 후 설치해야 합니다.
  - 이 명령어는 `pcre2`라는 패키지의 로컬 RPM 파일을 설치합니다.
  - Zabbix Agent가 실행되기 위해 필요한 라이브러리 중 하나입니다.

---

## 2. **Zabbix Agent 설치**
```bash
sudo rpm -ivh zabbix-agent-6.0.4-1.el7.x86_64.rpm
```
- **설명**: 
  - Zabbix Agent 설치를 위해 제공된 RPM 패키지를 설치합니다.
  - `-i`: 설치(install)를 의미합니다.
  - `-v`: 설치 과정의 자세한 정보를 출력합니다.
  - `-h`: 설치 진행 상태를 해시(`#`)로 표시합니다.

---

## 3. **호스트명 확인**
```bash
hostname
```
- **설명**: 
  - 현재 서버의 호스트명을 확인합니다.
  - Zabbix Agent의 설정 파일에서 이 호스트명을 사용해야 하므로 미리 확인합니다.

---

## 4. **Zabbix Agent 설정 파일 수정**
```bash
vi /etc/zabbix/zabbix_agentd.conf
```
- **설명**: 
  - Zabbix Agent의 설정 파일을 수정하기 위해 열어줍니다.
  - 아래 항목들을 수정해야 합니다:
    1. **Server**: Zabbix Server의 IP 주소 또는 호스트명을 설정합니다.
    2. **ServerActive**: 활성 모니터링용 Zabbix Server의 IP 주소를 설정합니다.
    3. **Hostname**: 이 Agent를 구분하기 위한 호스트명을 설정합니다.
  - 설정 예시:
    ```plaintext
    Server=192.168.0.100
    ServerActive=192.168.0.100
    Hostname=my-server-name
    ```

---

## 5. **Zabbix Agent 서비스 시작**
```bash
systemctl start zabbix-agent
```
- **설명**: 
  - Zabbix Agent 서비스를 시작합니다.
  - 성공적으로 시작되면, Zabbix Server가 이 Agent를 통해 데이터를 수집할 수 있습니다.

---

## 6. **Zabbix Agent 서비스 자동 시작 활성화**
```bash
systemctl enable zabbix-agent
```
- **설명**: 
  - 서버 재부팅 후에도 Zabbix Agent가 자동으로 시작되도록 설정합니다.

---

## 7. **Zabbix Agent 서비스 상태 확인**
```bash
systemctl status zabbix-agent
```
- **설명**: 
  - Zabbix Agent 서비스가 정상적으로 실행되고 있는지 확인합니다.
  - 출력 결과에서 `active (running)` 상태인지 확인합니다.

---

### 주의사항
- Zabbix Server와 Zabbix Agent 간의 통신이 원활히 이루어지려면 방화벽 설정에서 Zabbix Server의 포트를 허용해야 합니다.
- 기본적으로 Zabbix Agent는 포트 `10050`을 사용합니다.

--- 

위의 과정을 따라하면 폐쇄망 환경에서도 Zabbix Agent를 설치하고 설정할 수 있습니다.