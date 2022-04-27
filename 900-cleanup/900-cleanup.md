# 실습 리소스 정리

이 실습을 마치면 사용한 AWS 계정에 비용이 추가로 발생하지 않도록 사용한 리소스를 삭제해야 합니다.

1. 인그레스 자원들을 삭제합니다. 이 때, 해당 yaml 파일이 위치한 폴더(/home/ec2-user/environment/manifests)에서 아래의 명령어를 수행합니다.

```
cd ~/environment/manifests/

kubectl delete -f ingress.yaml
kubectl delete -f alb-ingress-controller/v2_4_1_full.yaml
```

2. 생성한 EKS 클러스터를 제거합니다.

```
eksctl delete cluster --name=eks-demo
```

[!] AWS CloudFormation 콘솔에서 관련 스택이 모두 삭제된 것을 확인합니다.

3. 생성한 ECR 저장소를 삭제합니다. 아래의 명령어를 통해, 생성한 저장소 리스트를 불러옵니다.

```
aws ecr describe-repositories

aws ecr delete-repository --repository-name demo-flask-backend --force
aws ecr delete-repository --repository-name demo-frontend --force
```

4. 수집했던 지표들을 삭제합니다.

```
aws logs describe-log-groups --query 'logGroups[*].logGroupName' --output table | \
awk '{print $2}' | grep ^/aws/containerinsights/eks-demo | while read x; do  echo "deleting $x" ; aws logs delete-log-group --log-group-name $x; done
```

```
aws logs describe-log-groups --query 'logGroups[*].logGroupName' --output table | \
awk '{print $2}' | grep ^/aws/eks/eks-demo | while read x; do  echo "deleting $x" ; aws logs delete-log-group --log-group-name $x; done
```

5. 생성한 Cloud9 IDE 환경을 삭제합니다.

[Previous](../110-cicd/200-cicd-security.md) | [Next](../README.md)