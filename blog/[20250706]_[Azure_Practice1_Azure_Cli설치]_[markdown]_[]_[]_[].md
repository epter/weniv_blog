---
# 윈도우 11 환경 기준 설치 메뉴얼

로컬 Powershell 을 이용해서 설치

---

## 📁 1. 설치 준비

인터넷이 되는 환경
- 참고 메뉴얼
- 공식 마이크로 소프트 사이트
- https://learn.microsoft.com/ko-kr/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=winget

---

## 🛠 2. 설치 방법 

### POWERSHELL 에서

```bash

winget install --exact --id Microsoft.AzureCLI

```

---

## 🖥 3. Azure Cli 로그인

```bash
az login
```
- 명령어 입력시 로그인 혹은 확인 팝업창 발생
---

## ⚙ 4. 삭제 방법

- 시작 > 설정 > 앱 > 설치된 앱 에서 해당 클리 파일 삭제
- C:\Users\<username>\.azure\msal_token_cache.bin 또는 C:\Users\<username>\.azure\msal_token_cache.json에서 해당 데이터를 제거합니다.
