#HTTP 메서드 속성
> 강의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 와 개인적으로 공부한 내용을 정리하였습니다.

![](images/http-method-table.png)

##안전(Safe Methods)
- 호출해도 리소스를 변경하지 않음 (ex: GET, HEAD)
- 리소스의 변경이 일어나지 않는 것을 Safe하다고 표현함

##멱등(Idempotent Methods)
- 몇 번을 호출하든 결과가 동일해야함
- 멱등 메서드
    - GET : 조회를 여러 번 해도 동일한 결과가 조회됨
    - PUT : 결과를 대체함. 같은 요청을 여러 번 수행해도 최종 결과는 같음
    - DELETE : 같은 요청을 여러 번 해도 삭제된 결과는 같음
- POST : 멱등이 아니다! 
    - 두 번 결제하면 중복 결제가 발생함
- 단, 멱등은 외부 요인으로 중간에 리소스가 변경된 것을 고려하지 않음
    - 사용자1: GET -> username:A, age:20
    - 사용자2: PUT -> username:A, age:30
    - 사용자1: GET -> username:A, age:30 -> 사용자2의 영향으로 바뀐 데이터 조회
    
#### 멱등은 어떨 때 필요할까? 자동 복구 메커니즘
- 예를 들어, 클라이언트가 자원 삭제 요청을 했는데 서버가 응답이 없을 경우, 요청을 여러 번 해도 멱등하다는 것을 클라이언트가 인지하고 있으므로 DELETE 재요청할 수 있음. 판단의 근거가 됨
    
##캐시가능(Cacheable Methods)
- 응답 결과 리소스를 캐시해서 사용해도 되는가? 
- GET, HEAD, POST, PATCH 캐시가능 
- 실제로는 GET, HEAD 정도만 캐시로 사용
- POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음