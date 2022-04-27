# AWS Fargate로 배포하기

## AWS Fargate로 pod 배포하기

1. 클러스터에 Fargate로 pod를 배포하기 위해서는 pod가 실행될 때 사용하는 하나 이상의 fargate profile을 정의해야 합니다. 즉, fargate profile이란 fargate로 pod를 생성하기 위한 조건을 명시해놓은 프로파일이라고 보시면 됩니다.

```
cd /home/ec2-user/environment/manifests
```

```
cat <<EOF> eks-demo-fargate-profile.yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: eks-demo
  region: ${AWS_REGION}
fargateProfiles:
  - name: frontend-fargate-profile
    selectors:
      - namespace: default
        labels:
          app: frontend-fargate
EOF
```

위의 yaml 파일에서 selectors에 기재된 조건에 부합하는 pod의 경우, fargate로 배포됩니다.

2. 아래의 명령어를 통해, fargate profile을 프로비저닝합니다.

```
eksctl create fargateprofile -f eks-demo-fargate-profile.yaml                         
```

3. Fargate profile이 정상적으로 생성되었는지 확인합니다.

```
eksctl get fargateprofile --cluster eks-demo -o json
```

```
# 결과 값 예시 
{
    "name": "frontend-fargate-profile",
    "podExecutionRoleARN": "arn:aws:iam::account-id:role/eksctl-eks-demo-test-farga-FargatePodExecutionRole-OLC3P21AD5DX",
    "selectors": [
        {
            "namespace": "default",
            "labels": {
                "app": "frontend-fargate"
            }
        }
    ],
    "subnets": [
        "subnet-07e2d55650225419c",
        "subnet-0ac4a7fdbd803039c",
        "subnet-046a3dcfabce11b5f"
    ],
    "status": "ACTIVE"
} 
```

우리는 [서비스 배포하기](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/70-deploy-service)에서 배포한 3개의 pod 중, 프론트앤드 pod를 fargate로 프로비저닝하는 작업을 수행하겠습니다. 먼저, 기존의 pod를 삭제하는 작업을 수행합니다. 아래의 명령어를 yaml 파일이 위치한 폴더에서 작업합니다.

```
kubectl delete -f frontend-deployment.yaml 
```

5. 그리고 frontend-deployment.yaml 파일을 수정합니다. 이전에 작성한 yaml 파일과 비교해보면 label의 value 값이 demo-frontend에서 frontend-fargate로 변경되었음을 확인할 수 있습니다. 1번에서 key 값이 app이고 value 값이 frontend-fargate이며 namespace가 default일 때, pod를 fargate로 배포하겠다는 조건을 맞추기 위해 값을 변경하였습니다.

```
cd /home/ec2-user/environment/manifests

cat <<EOF> frontend-deployment.yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-frontend
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend-fargate
  template:
    metadata:
      labels:
        app: frontend-fargate
    spec:
      containers:
        - name: demo-frontend
          image: $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-frontend:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 80
EOF
```

6. frontend-service.yaml 파일도 수정합니다.

```
cat <<EOF> frontend-service.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: demo-frontend
  annotations:
    alb.ingress.kubernetes.io/healthcheck-path: "/"
spec:
  selector:
    app: frontend-fargate
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
EOF
```

7. 매니페스트를 배포합니다.

```
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
```

8. 아래의 명령어를 통해, pod 리스트를 확인해보면 demo-frontend pod의 경우, fargate-ip-XX 노드에 프로비저닝되었음을 확인할 수 있습니다.

```
kubectl get pod -o wide
```

9. 또한, 아래 명령어 수행 결과를 웹 브라우저에 붙여넣으면 이전과 같은 화면을 확인할 수있습니다.

```
echo http://$(kubectl get ingress/backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')
```

[Previous](./80-fargate.md) | [Next](../90-monitoring/90-monitoring.md)