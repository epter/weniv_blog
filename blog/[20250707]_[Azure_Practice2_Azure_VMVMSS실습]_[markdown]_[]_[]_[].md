-----

## Azure 가상 머신(VM) 및 가상 머신 확장 집합(VMSS)을 이용한 웹 서버 구축 및 관리 연습

오늘은 **Microsoft Azure** 클라우드 플랫폼에서 \*\*가상 머신(VM)\*\*과 \*\*가상 머신 확장 집합(VMSS)\*\*을 활용하여 웹 서버를 구축하고 관리하는 과정을 직접 실습했습니다. 이 연습을 통해 클라우드 환경에서 유연하고 확장 가능한 인프라를 구성하는 방법을 익힐 수 있었습니다.

-----

### 1\. Azure 리소스 그룹 생성 및 단일 가상 머신(VM) 배포

클라우드 리소스들을 효율적으로 관리하기 위해 먼저 리소스 그룹을 생성했습니다.

```bash
az group create --name rg-free-tier-demo --location japaneast
```

이후 단일 웹 서버 역할을 할 가상 머신을 배포했습니다. Ubuntu 24.04 이미지를 사용하여 `Standard_B1s` (프리티어 가능) 크기로 **Japan East** 리전에 VM을 생성했습니다. SSH 키를 자동으로 생성하여 보안 접근을 설정했습니다.

```bash
az vm create \
  --resource-group rg-free-tier-demo \
  --name myWebVM \
  --image Ubuntu2404 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --size Standard_B1s \
  --location japaneast \
  --output json
```

### 2\. 가상 머신에 Nginx 웹 서버 설치 및 설정

생성된 VM에 SSH로 접속하여 웹 서버로 사용할 **Nginx**를 설치하고 활성화했습니다.

```bash
ssh azureuser@<VM Public IP> # VM 생성 후 출력되는 Public IP 주소로 대체
sudo apt update && sudo apt install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

Nginx 설치 후 웹 브라우저에서 접속하기 위해 VM의 80번 포트를 열어주었습니다.

```bash
az vm open-port --port 80 --resource-group rg-free-tier-demo --name myWebVM
```

이제 웹 브라우저에서 `http://<VM Public IP>`로 접속하면 Nginx 기본 페이지를 확인할 수 있습니다.

-----

### 3\. 가상 머신 확장 집합(VMSS)으로 확장성 확보

단일 VM은 트래픽 증가에 대한 유연한 대응이 어렵습니다. 이를 해결하기 위해 \*\*가상 머신 확장 집합(VMSS)\*\*을 생성하여 여러 VM 인스턴스를 자동으로 관리하고 확장할 수 있도록 했습니다. 이번에는 **Korea Central** 리전에 2개의 `Standard_B1s` 인스턴스로 VMSS를 구성했습니다.

```bash
az vmss create \
  --resource-group rg-free-tier-demo \
  --name myWebVMSS \
  --image Ubuntu2404 \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --vm-sku Standard_B1s \
  --location koreacentral \
  --instance-count 2 \
  --output json
```

VMSS 내의 모든 인스턴스에 Nginx를 자동으로 설치하기 위해 **Custom Script Extension**을 사용했습니다. 이는 VM이 시작될 때 특정 스크립트를 실행하도록 지시합니다.

```bash
# Nginx 설치 스크립트 설정 파일을 생성
@"
{
  "commandToExecute": "apt-get update && apt-get install -y nginx && systemctl enable nginx && systemctl start nginx"
}
"@ | Out-File -Encoding utf8 -FilePath .\install-nginx.json

# VMSS에 Custom Script Extension 적용
az vmss extension set \
  --resource-group rg-free-tier-demo \
  --vmss-name myWebVMSS \
  --name CustomScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings .\install-nginx.json
```

-----

### 4\. 리소스 정리

실습 완료 후 불필요한 비용 발생을 막기 위해 생성했던 모든 Azure 리소스들을 삭제했습니다. 리소스 그룹을 삭제하면 해당 그룹에 속한 모든 리소스가 함께 삭제됩니다.

```bash
az group delete --name rg-free-tier-demo --yes --no-wait
```

-----

### 마치며

이번 연습을 통해 Azure에서 VM과 VMSS를 활용하여 웹 서버 환경을 구축하는 기본적인 과정을 경험했습니다. 특히, VMSS를 통한 자동화된 배포와 확장은 클라우드 환경의 강력한 이점을 명확하게 보여주었습니다. 다음번에는 로드 밸런서와 자동 크기 조정 규칙을 VMSS에 적용하여 더욱 견고하고 트래픽 변화에 유연한 아키텍처를 구축하는 방법을 탐구해 볼 예정입니다.

클라우드 인프라 구축에 관심 있는 분들께 이 포스팅이 도움이 되기를 바랍니다\! 혹시 더 궁금한 점이나 공유하고 싶은 내용이 있다면 댓글로 남겨주세요.