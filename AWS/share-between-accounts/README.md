# AWS 계정들간에 자원 공유 방법 정리

### EC2
- AMI(이미지)를 다른 계정에 공유하는 방식
- 공유받은 이미지로 인스턴스를 띄우면 됨
- 공유해준 부모 계정의 키페어(pem)를 사용하고자 할 경우, 그냥 키페어 없이 생성하면 됨 

### RDS
- 스냅샷을 다른 계정에 공유하는 방식
- 공유받은 스냅샷으로 인스턴스를 띄우면 됨

### Route 53
- 도메인 공유 가능
- [방법 안내](Route53.md) 

### ACM
- ACM 자체는 공유되지 않으나, 다른 계정에서 활용할 수 있는 방법이 있음
- 만약 공유받을 도메인으로 ACM을 설정한다면, Route 53 설정을 먼저 진행하는 것이 좋음
- [방법 안내](ACM.md)

### VPN
- VPN도 타 계정간 공유 가능함