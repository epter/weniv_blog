
# Linux에서 Timezone 설정 가이드

Linux 시스템의 시간대를 설정하고 확인하는 방법을 정리했습니다.

---

## 1. **Timezone 설정**
```bash
timedatectl set-timezone Asia/Tokyo
```
- **설명**:
  - `timedatectl` 명령어를 사용해 시스템의 시간대를 설정합니다.
  - `Asia/Tokyo`는 일본 표준시간(JST)을 나타내는 시간대입니다.
  - 다른 시간대를 설정하려면 `/usr/share/zoneinfo` 디렉터리에서 사용 가능한 시간대 목록을 확인하거나 `timedatectl list-timezones` 명령어를 사용해 확인할 수 있습니다.

---

## 2. **Timezone 설정 확인**
```bash
timedatectl status
```
- **설명**:
  - 현재 시스템의 시간대 및 시간 정보를 확인합니다.
  - 출력 예시:
    ```plaintext
    Local time: Thu 2025-01-20 10:15:00 JST
    Universal time: Thu 2025-01-20 01:15:00 UTC
    RTC time: Thu 2025-01-20 01:15:00
    Time zone: Asia/Tokyo (JST, +0900)
    System clock synchronized: yes
    NTP service: active
    ```

---

### 주의사항
1. **Timezone 변경 후 효과**:
   - Timezone 설정 변경은 즉시 적용됩니다. 시스템 재부팅은 필요하지 않습니다.
   
2. **NTP 서비스**:
   - 시간 동기화(NTP 서비스)가 활성화된 경우, 정확한 시간 유지가 가능합니다.
   - 출력 결과의 `NTP service: active`로 확인할 수 있습니다.

3. **시간대 변경 권한**:
   - `timedatectl` 명령어를 사용하려면 **root 권한** 또는 **sudo 권한**이 필요합니다.

---

### 추가 명령어
- **사용 가능한 시간대 목록 확인**:
  ```bash
  timedatectl list-timezones
  ```
  - 이 명령어로 설정 가능한 시간대의 전체 목록을 출력합니다.
  
- **현재 시간 및 날짜만 확인**:
  ```bash
  date
  ```
  - 시스템의 현재 시간을 간단히 확인할 수 있습니다.

