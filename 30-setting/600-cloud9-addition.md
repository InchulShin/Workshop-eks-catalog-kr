# AWS Cloud9 추가 셋팅하기
앞선 페이지에서 AWS Cloud9 IDE 구축 및 필요한 툴 설치를 수행한 후, 아래의 추가 설정 작업을 진행합니다.

1. 현재 실습이 진행되고 있는 리전을 기본 값으로 하도록 aws cli를 설정합니다.

```
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')

echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
    
aws configure set default.region ${AWS_REGION}
```

설정한 리전 값을 확인합니다.

```
aws configure get default.region
```

2. 현재 실습을 진행하는 계정 ID를 환경 변수로 등록합니다.

```
export ACCOUNT_ID=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.accountId')

echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile
```

3. 도커 이미지를 빌드하는 동안, AWS Cloud9 환경에서 용량 부족 이슈가 발생할 수 있습니다. 이를 해결하기 위해, 디스크 사이즈를 증설하는 쉘 스크립트를 실행합니다.

```
wget https://gist.githubusercontent.com/joozero/b48ee68e2174a4f1ead93aaf2b582090/raw/2dda79390a10328df66e5f6162846017c682bef5/resize.sh
```

```
sh resize.sh
```

완료 후, 아래의 명령어를 통해 파일 시스템에 늘어난 볼륨 크기가 반영되었는지 확인합니다.

```
df -h
```

리전을 명시해주지 않으면 클러스터를 배포한 후, 관련 정보를 확인할 때 에러가 뜹니다.

[Previous](./500-eksctl.md) | [Next](../40-container/40-container.md)