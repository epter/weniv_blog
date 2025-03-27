# WSL에서 로그 파일을 이동하고 grep으로 특정 문자열 검색하기

## 개요
Windows에서 생성된 로그 파일을 WSL 환경으로 이동한 후, `grep`을 사용하여 특정 문자열을 포함한 로그를 검색하는 방법을 정리합니다.

## 1. Windows에서 WSL로 로그 파일 이동하기
Windows의 `C:\temp` 폴더에 저장된 로그 파일을 WSL의 `/etc/logchecker/` 폴더로 이동하는 방법은 다음과 같습니다.

### 1.1 WSL에서 Windows 파일 시스템 접근
WSL에서는 `C:\` 드라이브가 `/mnt/c/` 경로에 마운트됩니다. 따라서 Windows의 `C:\temp` 폴더에 있는 파일은 WSL에서 다음 경로로 접근할 수 있습니다.

```sh
ls /mnt/c/temp/
```

### 1.2 파일을 WSL의 특정 폴더로 이동
WSL에서 `mv` 명령을 사용하여 파일을 이동합니다.

```sh
mv /mnt/c/temp/*.log /etc/logchecker/
```

> `/etc/logchecker/` 폴더가 존재하지 않는다면 먼저 생성해야 합니다.
>
> ```sh
> mkdir -p /etc/logchecker/
> ```

## 2. grep을 사용하여 특정 문자열 검색하기
이제 이동한 로그 파일에서 특정 키워드(예: `err`)가 포함된 내용을 검색해 보겠습니다.

### 2.1 단순 검색
`grep` 명령을 사용하여 `err` 문자열이 포함된 모든 라인을 출력합니다.

```sh
grep 'err' /etc/logchecker/*.log
```

### 2.2 대소문자 구분 없이 검색
기본적으로 `grep`은 대소문자를 구분합니다. 대소문자 구분 없이 검색하려면 `-i` 옵션을 사용합니다.

```sh
grep -i 'err' /etc/logchecker/*.log
```

### 2.3 라인 번호와 함께 출력
검색된 문자열이 포함된 라인의 번호도 함께 표시하려면 `-n` 옵션을 추가합니다.

```sh
grep -in 'err' /etc/logchecker/*.log
```

### 2.4 특정 단어만 검색
`err`가 다른 단어의 일부가 아닌 독립적인 단어로 존재하는 경우만 검색하려면 `-w` 옵션을 사용합니다.

```sh
grep -iw 'err' /etc/logchecker/*.log
```

### 2.5 검색 결과를 다른 파일로 저장
검색된 내용을 `result.log` 파일로 저장하려면 `>` 리다이렉션을 사용합니다.

```sh
grep -i 'err' /etc/logchecker/*.log > /etc/logchecker/result.log
```

## 마무리
이제 Windows에서 생성된 로그 파일을 WSL로 이동한 후, `grep`을 사용하여 특정 문자열을 포함하는 로그를 효율적으로 검색할 수 있습니다. 추가적인 필터링이 필요하다면 `awk`나 `sed`와 같은 명령어를 활용할 수도 있습니다.

