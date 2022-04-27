# AWS CLI

## AWS CLI 업데이트하기
[AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) 는 command-line shell의 명령을 사용하여 AWS 서비스와 상호 작용할 수 있는 오픈 소스 툴입니다. AWS Cloud9는 기본적으로 AWS CLI가 설치되어 있습니다.

아래의 명령어를 통해, AWS CLI 버전을 업그레이드합니다.

```
sudo pip install --upgrade awscli
```

아래의 명령어를 통해, 버전을 확인합니다. 이때 1.18.124 >= version 혹은 2.0.42 >= version을 만족해야 합니다.

```
aws --version
```

[Previous](./100-aws-cloud9.md) | [Next](./300-kubectl.md)
