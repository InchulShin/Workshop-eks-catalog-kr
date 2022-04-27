# eksctl 설치하기

## eksctl 설치하기
Amazon EKS 클러스터를 배포하는 방식은 다양합니다. AWS 콘솔, CloudFormation, CDK, eksctl 및 Terraform 등이 그 예입니다.

본 실습에서는 eksctl을 사용하여 클러스터를 배포합니다.

![](../images/aws_cloud9_tool_01.png)

[eksctl](https://eksctl.io/) 이란 EKS 클러스터를 쉽게 생성 및 관리하는 CLI 툴입니다. Go 언어로 쓰여 있으며 CloudFormation 형태로 배포됩니다.

아래의 명령어를 통해, 최신의 eksctl 바이너리를 다운로드 합니다.

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```

바이너리를 /usr/local/bin으로 옮깁니다.

```
sudo mv -v /tmp/eksctl /usr/local/bin
```

아래의 명령어를 통해 설치 여부를 확인합니다.

```
eksctl version
```

[Previous](./400-etc.md) | [Next](./600-cloud9-addition.md)