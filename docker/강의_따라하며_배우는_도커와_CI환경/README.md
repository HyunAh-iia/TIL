# [강의] 따라하며 배우는 도커와 CI환경
> 인프런 강의 [따라하며 배우는 도커와 CI환경](https://www.inflearn.com/course/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%8F%84%EC%BB%A4-ci) 를 듣고 내용을 정리하였습니다.

### 목차 설명
1. [도커 기본](1_도커_기본.md)
    - 도커 사용 이유
    - 주요 개념(이미지, 컨테이너, 가상화 기술과의 차이)
    - 설치 가이드(Mac, Windows, Windows Home)
    - 도커를 사용했을 때의 아키텍처 구조 
2. [기본적인 도커 클라이언트 명령어 알아보기](2_기본적인_도커_클라이언트_명령어_알아보기.md)
    - 도커 이미지 내부 파일 구조 보기
    - 컨테이너들 나열하기
    - 도커 컨테이너의 생명 주기
    - Docker Stop vs Docker Kill
    - 컨테이너 삭제하기
    - 실행 중인 컨테이너에 명령어 전달
    - 레디스를 이용한 컨테이너 이해
    - 실행 중인 컨테이너에서 터미널 생활 즐기기
3. 직접 도커 이미지를 만들어 보기
    - 도커 이미지 생성하는 순서
    - Dockerfile 만들기
    - 도커 파일로 도커 이미지 만들기
    - 내가 만든 이미지 기억하기 쉬운 이름 주기
4. 도커를 이용한 간단한 Node.js 어플 만들기
    - Node.js 앱 만들기
    - Dockerfile 작성하기
    - Package.json 파일이 없다고 나오는 이유
    - 셍성한 이미지로 어플리케이션 실행 시 접근이 안되는 이유
    - Working Directory 명시해주기
    - 어플리케이션 소스 변경으로 다시 빌드하는 것에 대한 문제점
    - 어플리케이션 소스 변경으로 재빌드 시 효율적으로 하는 법
    - Docker Volumne에 대하여
5. Docker Compose
    - Docker Compose란 무엇인가?
    - 어플리케이션 소스 작성하기
    - Dockerfile 작성하기
    - Docker Containers 간 통신할 때 나타나는 에러
    - Docker Compose 파일 작성하기
    - Docker Compose로 컨테이너를 멈추기
6. 간단한 어플을 실제로 배포해보기(개발 환경 부분)
    - 리액트 앱 설치하기
    - 도커를 이용해 리액트 앱 실행하기
    - 생성된 도커 이미지로 리액트 앱 실행해보기
    - 도커 볼륨을 이용한 소스코드 변경
    - 도커 컴포즈로 좀 더 간단하게 앱 실행해보기
    - 리액트 앱 테스트하기
    - 운영환경을 위한 Nginx
    - 운영환경 도커 이미지를 위한 Dockerfile 작성하기
7. 간단한 어플을 실제로 배포해보기(테스트 & 배포 부분)
    - Travis CI 설명
    - Travis CI 이용 순서
    - .travis.yml 파일 작성하기(테스트까지)
    - AWS 알아보기
    - Elastic Beanstalk 환경 구성하기
    - .travis.yml 파일 작성하기 (배포 부분)
    - Travis CI의 AWS 접근을 위한 API 생성
8. 복잡한 어플을 실제로 배포해보기(개발 환경 부분)
    - Node JS 구성하기
    - React JS 구성하기
    - 리액트 앱을 위한 도커 파일 만들기
    - 노드 앱을 위한 도커 파일 만들기
    - DB에 관하여
    - MYSQL을 위한 도커 파일 만들기
    - NGINX를 위한 도커 파일 만들기
    - Docker Compose 파일 작성하기
    - Docker Volumn을 이용한 데이터베이스 유지하기
9. 복잡한 어플을 실제로 배포해보기(테스트 & 배포 부분)
    - 도커 환경의 MYSQL 부분 정리하기
    - Github에 소스코드 올리기
    - Travis CI Steps
    - .trivis.yml 파일 작성하기
    - Dockerrun.aws.json에 대하여
    - Dockerrun.aws.json 파일 작성하기
    - 다중 컨테이너 앱을 위한 Elastic beanstalk 환경 생성
    - VPC(virtual private cloud)와 Security Group 설정하기
    - MYSQL을 위한 AWS RDS 생성하기
    - Security Group 생성하기
    - Security Group 적용하기
    - EB와 RDS 소통을 위한 환경 변수 설정하기
    - travis.yml 파일 작성하기 (배포 부분)
    - Travis CI의 AWS 접근을 위한 API key 생성