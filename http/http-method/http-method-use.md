#HTTP 메서드 활용
> 강의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 와 개인적으로 공부한 내용을 정리하였습니다.

## 클라이언트에서 서버로 데이터 전송
####클라이언트에서 서버로 데이터 전달 방식은 크게 2가지
1. 쿼리 파라미터를 통한 데이터 전송
   - GET
   - 주로 정렬 필터(검색어)
2. 메시지 바디를 통한 데이터 전송
   - POST, PUT, PATCH
   - 회원 가입, 상품 주문, 리소스 등록, 리소스 변경

####클라이언트에서 서버로 데이터 전송 4가지 상황
1. 정적 데이터 조회
   - 이미지, 정적 텍스트 문서
   - 조회는 GET
   - 보통 쿼리파라미터 없이 리소스 경로를 단순하게 조회
2. 동적 데이터 조회
   - 주로 검색, 게시판 목록에서 정렬 필터(검색어)
   - 쿼리파라미터 사용하여 필터링, 정렬에 주로 활용
   - 조회 시 GET 쿼리파라미터 사용해 데이터 전달
3. HTML Form을 통한 데이터 전송
   - `Content-Type: application/x-www-form-urlencoded` 
   - URL 인코딩 (ex. abc김 -> abc%EA%B9%80)
   - HTML Form submit 시 POST 전송(저장) => 메시지 바디에 인코딩된 URL 데이터가 key=value 형식으로 들어감 (ex. username=kim&age=20)
        - 회원 가입, 상품 주문, 데이터 변경 등에 사용
   - GET 전송 시 쿼리 파라미터에 인코딩된 URL 데이터가 들어감(ex. GET /member?username=kim&age=20)
   - `Content-Type: multipart/form-data`
        - 파일 업로드와 같은 바이너리 데이터 전송 시 사용
        - 다른 종류의 여러 파일과 폼 내용 함께 전송 가능(그래새ㅓ 이름이 멀티파트)
   - HTML Form 전송은 GET, POST만 지원 
4. HTTP API를 통한 데이터 전송
   - 서버 to 서버(백엔드 시스템 통신) 
   - 앱 클라이언트(아이폰, 안드로이드) 
   - 웹 클라이언트
        - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
        - 예) React, VueJs 같은 웹 클라이언트와 API 통신 
   - POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
   - GET: 조회, 쿼리 파라미터로 데이터 전달
   - Content-Type: application/json을 주로 사용 (사실상 표준)
        - TEXT, XML, JSON 등등