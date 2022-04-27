# AWS Cloud9 시작하기
AWS Cloud9으로 실습 환경을 구축하는 순서는 아래와 같습니다.

- AWS Cloud9으로 IDE 구성

- IAM Role 생성

- IDE(AWS Cloud9 인스턴스)에 IAM Role 부여

- IDE에서 IAM 설정 업데이트


## AWS Cloud9으로 IDE 구성

1. [AWS Cloud9 콘솔창](https://console.aws.amazon.com/cloud9) 에 접속한 후, Create environment 버튼을 클릭합니다.

2. IDE 이름을 적은 후, Next step을 클릭합니다. 본 실습에서는 eks-workspace로 입력합니다.

3. 인스턴스 타입(instance type)을 other instance type 라디오 버튼을 클릭 후, t3.medium으로 선택합니다. 플랫폼(platform)의 경우, Amazon Linux 2 (recommended) 를 선택하고, Next step을 클릭하여 지정한 속성 값을 확인한 후, Create environment를 클릭합니다.

4. 생성이 완료되면 아래와 같은 화면이 나타납니다.

![](../images/aws_cloud9_01.png)

AWS Cloud9의 경우, third-party-cookies를 필요로 합니다. 만약 위와 같은 화면이 나오지 않을 경우, [여기](https://docs.aws.amazon.com/cloud9/latest/user-guide/troubleshooting.html#troubleshooting-env-loading) 를 살펴봅니다.

5. IAM Role 생성

IAM Role은 특정 권한을 가진 IAM 자격 증명입니다. IAM 역할의 경우, IAM 사용자 및 AWS가 제공하는 서비스에 사용할 수 있습니다. 서비스에 IAM Role을 부여할 경우, 서비스가 사용자를 대신하여 수임받은 역할을 수행합니다.

본 실습에서는 Administrator access 정책을 가진 IAM Role을 생성하여 AWS Cloud9에 붙입니다.

1. [여기](https://console.aws.amazon.com/iam/home#/roles$new?step=type&commonUseCase=EC2%2BEC2&selectedUseCase=EC2&policies=arn:aws:iam::aws:policy%2FAdministratorAccess) 를 클릭하여 IAM Role 페이지에 접속합니다.

2. AWS service 및 EC2가 선택된 것을 확인하고 Next: Permissions를 클릭합니다.

3. AdministratorAccess 정책이 선택된 것을 확인하고 Next: Tags를 클릭합니다.

4. 태그 추가(선택 사항) 단계에서 Next: Review를 클릭합니다.

5. Role name에 eksworkspace-admin을 입력한 후, AdministratorAccess 관리형 정책이 추가된 것을 확인하고 Create role을 클릭합니다.

![](../images/aws_cloud9_02.png)

본 실습의 경우, AdministratorAccess 정책을 사용하지만 실제 프로덕션 환경을 구동할 때에는 최소 권한을 부여하는 것이 적합합니다.

## IDE(AWS Cloud9 인스턴스)에 IAM Role 부여
AWS Cloud9 환경은 EC2 인스턴스로 구동됩니다. 따라서 EC2 콘솔에서 AWS Cloud9 인스턴스에 방금 생성한 IAM Role을 부여합니다.

1. [여기](https://console.aws.amazon.com/ec2/v2/home?#Instances:sort=desc:launchTime) 를 클릭하여 EC2 인스턴스 페이지에 접속합니다.

2. 해당 인스턴스를 선택 후, Actions > Security > Modify IAM Role을 클릭합니다.

![](../images/aws_cloud9_03.png)

3. IAM Role에서 eksworkspace-admin을 선택한 후, Save 버튼을 클릭합니다.

![](../images/aws_cloud9_04.png)

## IDE에서 IAM 설정 업데이트
AWS Cloud9의 경우, IAM credentials를 동적으로 관리합니다. 해당 credentials는 EKS IAM authentication과 호환되지 않기에 이를 비활성화하고 IAM Role을 붙입니다.

1. AWS Cloud9 콘솔창에서 생성한 IDE로 다시 접속한 후, 우측 상단에 기어 아이콘을 클릭한 후, 사이드 바에서 AWS SETTINGS를 클릭합니다.

2. Credentials 항목에서 AWS managed temporary credentials 설정을 비활성화합니다.

3. Preference tab을 종료합니다.

![](../images/aws_cloud9_05.png)

4. Temporary credentials이 없는지 확실히 하기 위해 기존의 자격 증명 파일도 제거합니다.

```
rm -vf ${HOME}/.aws/credentials
```

5. GetCallerIdentity CLI 명령어를 통해, Cloud9 IDE가 올바른 IAM Role을 사용하고 있는지 확인하세요. 결과 값이 나오면 올바르게 설정된 것입니다.

```
aws sts get-caller-identity --query Arn | grep eksworkspace-admin
```

[Previous](./30-setting.md) | [Next](./200-aws-cli.md)
