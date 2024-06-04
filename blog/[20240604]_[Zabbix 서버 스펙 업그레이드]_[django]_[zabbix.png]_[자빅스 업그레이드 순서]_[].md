# zabbix 스펙 업데이트 순서

* 전체 작업 순서
1. 멘테넌스 모드 설정
2. 서버 종료
3. 하드웨어/클라우드 콘솔상 업그레이드
4. 서버 기동
5. Zabbix 서비스 재기동
6. 멘테넌스 모드 해제


1. 멘테넌스 모드 설정
    1. Zabbix 웹 인터페이스에 로그인
    2. Configuration -> Maintenance
    3. Create maintenance period 버튼
    4. 유지보수 기간의 설정을 다음과 같이 구성
        * Name: Zabbix Server Upgrade
        * Maintenance type: With data collection (데이터 수집을 계속하되 알림을 중지)
        * Active since: 현재 날짜의 10:00
        * Active till: 현재 날짜의 12:00
        * Description: Zabbix 서버 업그레이드 및 재기동
        * Hosts 탭에서 모든 호스트를 선택하거나 특정 호스트 그룹을 선택합니다.
        * Add 버튼을 클릭하여 유지보수 기간을 저장합니다.
        
    1. 뎁스1
        1. 뎁스2
            1. 뎁스3
                1. 뎁스4r