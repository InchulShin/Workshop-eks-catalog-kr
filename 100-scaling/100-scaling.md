# Autoscaling Pod & Cluster

## 쿠버네티스 오토 스케일링에 대하여
오토 스케일링 서비스는 사용자가 정의한 주기 및 이벤트에 따라 서버를 자동으로 생성하거나 삭제하는 기능을 의미합니다. 오토 스케일링을 사용함으로써 애플리케이션은 트래픽에 따라 탄력적으로 대응할 수 있습니다.

쿠버네티스에는 크게 두 가지의 오토 스케일링 기능이 있습니다.

- HPA(Horizontal Pod AutoScaler)

- Cluster Autoscaler

HPA는 CPU 사용량 또는 사용자 정의 메트릭을 관찰하여 파드 개수를 자동으로 스케일합니다. 그러나 해당 파드가 올라가는 EKS 클러스터 자체 자원이 모자라게 되는 경우, Cluster Autoscaler를 고려해야 합니다.

이러한 오토 스케일링 기능을 클러스터에 적용하면 보다 탄력적이고 확장이 가능한 환경을 구성할 수 있습니다.

[Previous](./100-scaling.md) | [Next](./100-pod-scaling.md)
