# AWS 계정들끼리 ACM 공유
> 참고 : https://aws.amazon.com/ko/premiumsupport/knowledge-center/acm-export-certificate/
- ACM 자체는 공유되지 않으나, 다른 계정에서 동일 ACM 인증서를 활용할 수 있는 방법이 있음
- 만약 공유받을 도메인으로 ACM을 설정한다면, Route 53 설정을 먼저 진행하는 것이 좋음

### 목표
- prod 계정에 `example.com` 도메인으로 ACM 인증서 존재하는 상태
- dev 계정에 `dev.example.com` 도메인으로 ACM 인증서 공유

### 1. dev 계정에 로그인하여 ACM 접속
- 인증서 요청 -> 공인인증서 요청
- 도메인 이름 추가
  - 하위 도메인명(dev.example.com)
  - * 도메인 기재(*.dev.example.com)
### 2. 검증방법 선택
- 참고 : [domain-ownership-validation](https://docs.aws.amazon.com/acm/latest/userguide/domain-ownership-validation.html)
- 여기에서는 ACM으로 인증할 것이므로, DNS 검증방식을 선택함

### 3. 등록한 도메인으로 인증서 검증 요청
- 30분 정도 소요된다는 메시지가 뜨지만, 정상적으로 설정하면 10분 이내로 확인 가능했음
- 설정 오류 시 1시간이 되도 보류 상태임

### 4. 도메인 검증
```
dig 하위도메인
```