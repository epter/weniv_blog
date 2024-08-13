# CentOS 7에서 유저 추가, 비밀번호 변경 및 root 권한 부여 방법

CentOS 7에서 새로운 사용자를 추가하고, 비밀번호를 설정하며, 필요에 따라 해당 사용자에게 root 권한을 부여하는 방법을 설명합니다.

## 1. 유저 추가
CentOS 7에서 새로운 사용자를 추가하려면 `useradd` 명령어를 사용합니다.

```bash
sudo useradd username
```

여기서 `username`은 추가하고자 하는 사용자의 이름입니다. 예를 들어 `newuser`라는 사용자를 추가하려면 다음과 같이 입력합니다:

```bash
sudo useradd newuser
```

## 2. 비밀번호 변경
새로 추가한 사용자 또는 기존 사용자의 비밀번호를 설정하거나 변경하려면 `passwd` 명령어를 사용합니다.

```bash
sudo passwd username
```

예를 들어, `newuser` 사용자의 비밀번호를 설정하려면 다음과 같이 입력합니다:

```bash
sudo passwd newuser
```

시스템이 새로운 비밀번호를 입력하라는 메시지를 표시하며, 비밀번호를 입력하면 됩니다.

## 3. Root 권한 부여
특정 사용자에게 root 권한을 부여하려면, 해당 사용자를 `wheel` 그룹에 추가해야 합니다. `wheel` 그룹은 CentOS 7에서 관리자 권한을 가진 사용자 그룹입니다.

```bash
sudo usermod -aG wheel username
```

예를 들어, `newuser`에게 root 권한을 부여하려면 다음과 같이 입력합니다:

```bash
sudo usermod -aG wheel newuser
```

이제 `newuser`는 `sudo` 명령어를 사용하여 관리자 권한으로 명령을 실행할 수 있습니다.

## 추가로 알면 좋은 내용: 유저 삭제 및 정보 확인

### 4. 유저 삭제
더 이상 필요하지 않은 사용자를 삭제하려면 `userdel` 명령어를 사용합니다. 이때 사용자의 홈 디렉토리와 관련 파일도 삭제하려면 `-r` 옵션을 추가합니다.

```bash
sudo userdel -r username
```

예를 들어, `newuser`를 삭제하려면 다음과 같이 입력합니다:

```bash
sudo userdel -r newuser
```

### 5. 유저 정보 확인
시스템에 존재하는 사용자의 정보를 확인하려면 `id` 명령어를 사용할 수 있습니다.

```bash
id username
```

예를 들어, `newuser`의 정보를 확인하려면 다음과 같이 입력합니다:

```bash
id newuser
```

이 명령어는 사용자의 UID, GID, 소속 그룹 등을 출력합니다.

## 결론

이 게시물에서는 CentOS 7에서 사용자를 추가하고, 비밀번호를 설정하며, root 권한을 부여하는 방법을 알아보았습니다. 또한 사용자를 삭제하고 정보를 확인하는 방법도 추가로 설명했습니다. 이 정보를 통해 시스템 관리에 필요한 기본적인 사용자 관리 작업을 수행할 수 있습니다.
