
# Zabbix에서 PSK 인증을 사용하여 Windows 서버 인증하기

## 1. Zabbix 서버에서 PSK 키 생성

먼저 Zabbix 서버에서 PSK(Pre-Shared Key)를 생성해야 합니다. 다음 명령을 사용하여 256비트의 PSK를 생성합니다:

```bash
openssl rand -hex 32 > /etc/zabbix/zabbix_agentd.psk
```

PSK가 저장된 파일의 경로를 기록해 둡니다.

## 2. Zabbix 서버 컨피그 수정

생성한 PSK를 Zabbix 서버에서 사용할 수 있도록 Zabbix Agent의 설정 파일을 수정합니다.

```bash
sudo nano /etc/zabbix/zabbix_agentd.conf
```

파일의 아래 부분에 다음 내용을 추가합니다:

```ini
TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=MyZabbixPSK
TLSPSKFile=/etc/zabbix/zabbix_agentd.psk
```

- `TLSConnect`와 `TLSAccept`는 PSK 방식을 사용하도록 설정합니다.
- `TLSPSKIdentity`는 PSK 식별자를 지정합니다. 이 값은 클라이언트와 동일해야 합니다.
- `TLSPSKFile`은 생성한 PSK 파일의 경로를 지정합니다.

변경 후 Zabbix Agent를 재시작합니다:

```bash
sudo systemctl restart zabbix-agent
```

## 3. 대상 Windows 서버에서 PSK 등록

Windows 서버에서 Zabbix Agent가 설치된 상태에서 PSK 설정을 진행합니다.

1. Zabbix Agent 설정 파일(`zabbix_agentd.conf`)을 엽니다. 보통 설치 경로는 `C:\Program Files\Zabbix Agent\` 입니다.
2. 아래의 설정을 추가합니다:

```ini
TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=MyZabbixPSK
TLSPSKFile=C:\zabbix_agentd.psk
```

3. Zabbix 서버에서 생성한 PSK 키를 `C:\zabbix_agentd.psk` 파일로 저장합니다. 

4. Zabbix Agent 서비스를 재시작합니다:

```powershell
Restart-Service -Name "Zabbix Agent"
```

## 4. Zabbix 웹 인터페이스에서 호스트 설정

Zabbix 웹 인터페이스에서 대상 호스트에 PSK 인증을 설정합니다.

1. Zabbix 웹 인터페이스에 로그인하고 `Configuration -> Hosts`로 이동합니다.
2. 대상 호스트를 선택하고, `Encryption` 탭으로 이동합니다.
3. `Connections to host`와 `Connections from host`를 모두 `PSK`로 설정합니다.
4. `PSK identity`를 `MyZabbixPSK`로 설정하고, `PSK`에 `zabbix_agentd.psk` 파일의 내용을 붙여넣습니다.
5. 저장합니다.

## 5. 암호화 인증이 안된 상태에서 암호화를 추가하는 시나리오

### 시나리오 개요

기존에 PSK를 사용하지 않는 상태에서 PSK 인증을 추가하려는 경우, 다음 절차를 따르면 됩니다.

1. **PSK 키 생성:** 위의 1단계와 동일하게 PSK 키를 생성합니다.
2. **기존 호스트 영향 확인:** 현재 인증되지 않은 통신을 사용 중인 호스트들에 대해 인증 추가 시 문제가 없는지 확인합니다.
3. **서버 측 설정:** Zabbix 서버에서 PSK 설정을 추가하고, Zabbix Agent를 재시작합니다.
4. **클라이언트 측 설정:** 대상 Windows 서버에서도 PSK 설정을 추가하고 Zabbix Agent를 재시작합니다.
5. **Zabbix 웹 설정 변경:** 웹 인터페이스에서 해당 호스트의 암호화 설정을 PSK로 변경합니다.
6. **문제 발생 시 대처:** PSK 설정 적용 후 통신 오류가 발생하면 설정을 되돌리고 로그를 확인하여 문제를 해결합니다.

### 주의사항

- 기존 호스트와의 통신이 모두 안전하게 유지되도록 점진적으로 설정을 적용합니다.
- PSK 설정 후 인증에 실패할 경우, Zabbix 서버와 클라이언트의 로그를 확인하여 문제를 파악합니다.

