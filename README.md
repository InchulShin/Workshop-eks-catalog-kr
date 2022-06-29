# DEMOGO - CATS AND DOGS

## 사랑과 행복을 찾아서

![](./images/catsdogs.svg)

catsdogs이 워크샵은 Amazon ECS, Fargate 또는 도커 컨테이너의 워크플로우에 친숙하지 않은 분들도 쉽게 따라할 수 있습니다.

## Amazon ECS
Amazon Elastic Container Service(Amazon ECS)는 완전관리형 컨테이너 오케스트레이션 서비스입니다. 기본적으로 Amazon Route 53, Secrets Manager, AWS Identity and Access Management(IAM), Amazon CloudWatch 등의 다른 서비스와 통합을 통해 컨테이너 배포 및 확장을 위한 익숙한 환경을 제공할 수 있습니다. 다른 AWS 서비스와의 신속한 통합을 통해 ECS에 새로운 기능을 추가할 수도 있습니다. ECS를 통해 애플리케이션에서 Amazon EC2 및 AWS Fargate를 스팟 및 온디맨드 요금 옵션과 조합하여 유연하게 사용할 수도 있습니다.

## 실습 목적
이 워크샵을 통해 Amazon ECS의 기본 개념과 주요 구성 요소들을 이해합니다.
Amazon ECS 관리에 활용할 수 있는 유용한 도구들을 실습합니다.

### 다루는 AWS 서비스
- Amazon Elastic Container Service (ECS)
- Amazon Elastic Container Resistry (ECR)
- AWS Fargate
- AWS FireLens
- Amazon CloudWatch
- Auto Scaling
- AWS Identity and Access Management (IAM)
- Amazon CloudFormation
- Amazon EC2
- Amazon Application Load Balancer (ALB)

실습 순서
1. [실습 소개](./1.-intro/1.-intro.md)
2. [실습 환경 구성](./2.-setup/2.-setup.md)
3. [AMAZON ECR](./3.-ecr/3.-ecr.md)
4. [AMAZON ECS](./4.-ecs/4.-ecs.md)
5. [모니터링](./5.-monitoring/5.-monitoring.md)
6. [오토스케일링](./6.-autoscale/6.-autoscale.md)
7. [실습 결과](./7.-conclusion/7.-conclusion.md)
8. [지속적 통합 및 배포](./8.-cicd/8.-cicd.md)
    1. [지속적 통합 및 배포2](./8.-cicd/8.-cicd-2.md)
9. [실습 리소스 정리](./9.cleanup/9.cleanup.md)

<!--
[원본](http://ecs.catsdogs.kr.s3-website.ap-northeast-2.amazonaws.com/ko/)
-->
