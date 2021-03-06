#HTTP API 설계
> 강의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 와 개인적으로 공부한 내용을 정리하였습니다.

`Resource(리소스)`라고 표현하고 있지만 최근에는 `Representation`이라는 표현으로 변경됨.

#### 가장 중요한 것은 리소스 식별 (API URI 고민)
- URI(Uniform Resource Identifier)
- 리소스의 의미는 뭘까?
    - 회원을 등록하고 수정하고 조회하는게 리소스가 아니다! 예) 미네랄을 캐라 -> 미네랄이 리소스
    - 회원이라는 개념 자체가 바로 리소스다.
- 리소스를 어떻게 식별하는게 좋을까?
    - 회원을 등록하고 수정하고 조회하는 것을 모두 배제
    - 회원이라는 리소스만 식별하면 된다. -> 회원 리소스를 URI에 매핑
- 예 (회원관리)
    - 회원 목록 조회 /members
    - 회원 조회 /members/{id} -> 어떻게 구분하지? 
    - 회원 등록 /members/{id} -> 어떻게 구분하지? 
    - 회원 수정 /members/{id} -> 어떻게 구분하지? 
    - 회원 삭제 /members/{id} -> 어떻게 구분하지?
    
#### 리소스와 행위를 식별
- URI는 리소스만 식별!
- 리소스와 해당 리소스를 대상으로 하는 행위을 분리
- 리소스: 회원
- 행위: 조회, 등록, 삭제, 변경
- 리소스는 명사, 행위는 동사 (미네랄을 캐라) 행위(메서드)는 어떻게 구분?
    => method에 대한 글 정리 ㄱㄱ
    
#### HTTP 주요 메서드 종류
- GET: 리소스 조회
- POST: 요청 데이터 처리, 주로 등록에 사용
- PUT: 리소스를 대체, 해당 리소스가 없으면 생성
- PATCH: 리소스 부분 변경
- DELETE: 리소스 삭제

#### HTTP 기타 메서드 종류
- HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용) 
- CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정
- TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

# HTTP API 설계 예시
#### 회원 관리 시스템(POST 기반 등록) - 컬렉션
```
회원 목록 /members -> GET
회원 등록 /members -> POST
회원 조회 /members/{id} -> GET
회원 수정 /members/{id} -> PATCH, PUT, POST 
회원 삭제 /members/{id} -> DELETE
```
- 클라이언트는 등록될 리소스의 URI를 모른다.
    - 회원 등록 `POST /members`
- 서버가 새로 등록된 리소스 URI를 생성해준다.
    - `HTTP/1.1 201 Created  Location: /members/100`
- 컬렉션(Collection)
    - 서버가 관리하는 리소스 디렉토리 
    - 서버가 리소스의 URI를 생성하고 관리 
    - 여기서 컬렉션은 /members
#### 파일 관리 시스템(PUT 기반 등록) - 스토어
```
파일 목록 /files -> GET
파일 조회 /files/{filename} -> GET 
파일 등록 /files/{filename} -> PUT 
파일 삭제 /files/{filename} -> DELETE 
파일 대량 등록 /files -> POST
```
- 클라이언트가 리소스 URI를 알고 있어야 한다.
    - 파일 등록 PUT /files/{filename}
    - PUT /files/star.jpg
- 클라이언트가 직접 리소스의 URI를 지정한다. 
- 스토어(Store)
    - 클라이언트가 관리하는 리소스 저장소 
    - 클라이언트가 리소스의 URI를 알고 관리 
    - 여기서 스토어는 /files

#### 클라에서 HTML FORM만 사용할 수 있을 때 - 컨트롤 URI
```
회원 목록 -> GET /members
회원 등록 폼 -> GET /members/new
회원 등록 -> POST /members/new, POST /members
회원 조회 -> GET /members/{id}
회원 수정 폼 -> GET /members/{id}/edit
회원 수정 -> POST /members/{id}/edit, POST /members/{id}
회원 삭제 -> POST /members/{id}/delete
```
- HTML FORM은 GET, POST만 지원
- AJAX 같은 기술을 사용해서 해결 가능(POST 기반 컬렉션 회원 API 참고) -> 여기서는 순수 HTML, HTML FORM 이야기함
- GET, POST만 지원하므로 제약이 있음
- 컨트롤 URI
    - 이런 제약을 해결하기 위해 동사로 된 리소스 경로 사용 
    - POST의 /new, /edit, /delete가 컨트롤 URI
    - HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)
#### 정리
- HTTP API - 컬렉션
    - POST 기반 등록
    - 서버가 리소스 URI 결정 
- HTTP API - 스토어
    - PUT 기반 등록
    - 클라이언트가 리소스 URI 결정 
- HTML FORM 사용
    - 순수 HTML + HTML form 사용
    - GET, POST만 지원
#### 참고하면 좋은 URI 설계 개념
- 문서(document)
    - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
    - 예) /members/100, /files/star.jpg
- 컬렉션(collection)
    - 서버가 관리하는 리소스 디렉토리
    - 서버가 리소스의 URI를 생성하고 관리
    - 예) /members, 회원관리시스템
- 스토어(store)
    - 클라이언트가 관리하는 자원 저장소 클라이언트가 리소스의 URI를 알고 관리 예) /files
    - 예) /files, 파일관리시스템
- 컨트롤러(controller), 컨트롤 URI
    - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
    - 동사를 직접 사용 
    - 예) /members/{id}/delete
