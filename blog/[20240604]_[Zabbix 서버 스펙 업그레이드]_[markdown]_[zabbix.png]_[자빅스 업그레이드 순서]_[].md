# Zabbix 서버 업그레이드 및 재기동 절차

## 1. 유지보수 모드 설정 (Zabbix 웹 인터페이스)
1. Zabbix 웹 인터페이스에 로그인합니다.
2. **Configuration** 탭을 클릭한 후 **Maintenance**를 선택합니다.
3. **Create maintenance period** 버튼을 클릭합니다.
4. 유지보수 기간 설정:
    - **Name**: Zabbix Server Upgrade
    - **Maintenance type**: With data collection
    - **Active since**: 현재 날짜의 10:00
    - **Active till**: 현재 날짜의 12:00
    - **Description**: Zabbix 서버 업그레이드 및 재기동
5. **Hosts** 탭에서 모든 호스트를 선택하거나 특정 호스트 그룹을 선택합니다.
6. **Add** 버튼을 클릭하여 유지보수 기간을 저장합니다.

## 2. 서버 종료 (Rocky Linux)
1. 터미널을 열고 Zabbix 서버를 안전하게 종료합니다:
    ```bash
    sudo systemctl stop zabbix-server
    sudo systemctl stop zabbix-agent
    sudo systemctl stop mariadb  # MariaDB 사용하는 경우
    ```
2. 서버를 종료합니다:
    ```bash
    sudo shutdown -h now
    ```

## 3. 하드웨어 업그레이드 (CPU 및 메모리 추가)
1. 서버의 물리적인 CPU 및 메모리를 추가합니다.
2. 하드웨어 업그레이드가 완료되면 서버를 다시 켭니다.

## 4. 서버 시작 (Rocky Linux)
1. 서버가 다시 켜진 후 터미널에 로그인합니다.
2. Zabbix 서버와 관련된 서비스가 자동으로 시작되지 않았다면 수동으로 시작합니다:
    ```bash
    sudo systemctl start mariadb  # MariaDB 사용하는 경우
    sudo systemctl start zabbix-server
    sudo systemctl start zabbix-agent
    ```

## 5. Zabbix 서버 재기동 (Rocky Linux)
1. Zabbix 서버의 상태를 확인합니다:
    ```bash
    sudo systemctl status zabbix-server
    sudo systemctl status zabbix-agent
    sudo systemctl status mariadb  # MariaDB 사용하는 경우
    ```

## 6. 유지보수 모드 해제 (Zabbix 웹 인터페이스)
1. Zabbix 웹 인터페이스에 로그인합니다.
2. **Configuration** 탭을 클릭한 후 **Maintenance**를 선택합니다.
3. **Zabbix Server Upgrade** 유지보수 기간을 선택합니다.
4. **Delete** 버튼을 클릭하여 유지보수 기간을 삭제합니다.

이 단계를 따르면 Zabbix 서버의 재기동 동안 알림을 방지하고 하드웨어 업그레이드를 성공적으로 완료할 수 있습니다.
