
PsExec를 사용할 때 `-i` 옵션을 사용해 특정 세션에서 작업을 실행해야 할 때가 많습니다. 하지만 **세션 번호를 정확히 지정하지 않으면 실행이 실패**할 수 있습니다. 특히 원격 서버에서 사용자의 세션 번호를 자동으로 지정하지 않는 경우 직접 세션 번호를 확인하고 지정해 주어야 합니다.

#### 문제 상황

- PsExec 명령어에 `-i` 옵션을 사용할 때, 세션 번호를 지정하지 않거나 잘못 지정하면 **원격 실행이 실패**할 수 있습니다.
- 예를 들어 `PsExec -i 2 powershell.exe`처럼 지정할 때 세션 번호 `2`가 없거나 올바르지 않다면, 스크립트가 실행되지 않습니다.

#### 해결 방법

1. **세션 번호 확인**: `query user` 명령어를 사용해 현재 세션 목록을 확인합니다. PowerShell에서 실행 예시는 다음과 같습니다:

    ```powershell
    $sessionId = (query user | Select-String "사용자 이름" | ForEach-Object { ($_ -split '\s+')[-2] })
    ```

    여기서 필요한 세션 번호를 변수에 저장할 수 있습니다.

2. **PsExec에 세션 번호 지정하여 실행**: 위에서 확인한 세션 번호를 `-i` 옵션에 사용하여 PsExec 명령어를 실행합니다.

    ```powershell
    psexec \\원격서버 -u 사용자 -p 비밀번호 -h -i $sessionId powershell.exe -ExecutionPolicy Bypass -File "D:\temp\script.ps1"
    ```

### 요약

1. `query user` 명령어로 세션 번호를 조회합니다.
2. 조회한 세션 번호를 `-i` 옵션에 지정하여 PsExec 명령어를 실행하면 문제를 해결할 수 있습니다.

이렇게 하면 PsExec에서 세션 번호를 제대로 지정하여 원격 작업을 실행할 수 있습니다.