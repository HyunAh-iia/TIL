#Nginx 기본 환경 설정
Nginx는 환경 설정 텍스트 파일로 여러 가지 값을 지정해 Nginx 설정을 할 수 있도록 지원한다. Nginx 설치 시 기본적으로 설정하는 환경설정 값들을 알아보겠다.

#### 참고 링크
아래 블로그 글이 도움이 많이 되었다. Nginx 구조, 환경설정 지시어의 의미, 지시어별 권장 수치, 표준 설정과 서버 성능 테스트 방법도 제안하고 있다.
- [[Nginx] 엔진엑스 기본 환경 설정](https://12bme.tistory.com/366)
- [Nginx proxy & Linux 최적화 셋팅](https://www.burndogfather.com/190)
- [NGINX 기본 환경 설정 튜닝 및 설명](https://extrememanual.net/9976)
- [가상호스팅이란?](https://opentutorials.org/module/384/4529)
- [Ubuntu 16.04에 Nginx 설치하기](https://ndb796.tistory.com/341)

## 기본 설정
- nginx.conf : 어플리케이션의 기본 환경 설정 아래 명령어를 이용해 환경 파일을 찾을 수 있다.
```
find / -name nginx.conf
```

- 보통 `/etc/nginx/*` 아래에 설정파일이 위치해있고, 로그파일은 `/var/log/nginx/*` 에 위치해있다.

##### 간단한 뼈대부터 확인하자
```
worker_processes  1;
events {
    worker_connections  1024;
}
http { 
    include       mime.types;
    server {
        listen       80;
        location / {
            root   html;
            index  index.html index.htm;
        }
    }
}
```

아래와 같이 네 가지 영역으로 구성되어 있다.
1. Core 모듈
    코어 모듈은 대부분 환경 설정 파일의 최상단에 위치하며 한번만 사용할 수 있다. nginx의 기본적인 동작 방식을 정의한다.

2. http 블록
    웹서버에 대한 동작을 설정하는 영역으로, server 블록과 location 블록의 루트 블록이다.

3. server 블록
    하나의 웹사이트를 선언하는 데 사용된다. 가상 호스팅(Virtual Host)의 개념이다. 

4. location 블록
    server 블록 내에서 특정 URL을 처리하는 방법을 정의한다.

5. events
    주로 네트워크 동작에 관련된 설정하는 영역으로, 이벤트 모듈을 사용한다.

#### Nginx 설정 예제
NGINX를 설치하면 기본적으로 아래와 같이 설정이 되어있다. 여기에서 변경이 일어난 부분이나 부연설명이 필요한 부분에는 `##`으로 주석을 남겼다.
```
user  nginx;         ## NGINX 프로세스가 실행되는 권한, root 권한은 보안상 위험함
worker_processes  2; ## Default: 1, CPU 코어 하나에 최소한 한 개의 프로세스가 배정되도록 변경 권장
worker_priority   0; ## 값이 작을 수록 높은 우선순위를 갖는다. 커널 프로세스의 기본 우선순위인 -5 이하로는 설정하지 않도록 한다.

# 로그레벨 [ debug | info | notice | warn | error | crit ]
error_log  /var/log/nginx/error.log error; ## 로그레벨을 warn -> error로 변경함
pid        /var/run/nginx.pid;

events {
    worker_connections  1024; ## Default: 1024, 현 서버는 RAM 8GB라 상향조정
    multi_accept         off; ## Default: off
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

		server_tokens     off; ## 헤더에 NGINX 버전을 숨김 (보안상 설정 권장)
    keepalive_timeout  65; ## 접속 시 커넥션 유지 시간

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

## 리버스 프록시
따라서 /etc/nginx/site-available/default의 server 항목을 다시 수정한다. 총 2개의 server 항목이 존재할 수 있도록 만든다.  아래 코드와 같이 기본적으로 443 포트를 사용하도록 하고, 추가적으로 80번 포트로의 접속은 https로 리다이렉트 시키도록 작업하면 된다.
```
server {
        listen                  443;
        server_name             {도메인 주소 1} {도메인 주소 2};
        ssl                     on;
        ssl_certificate         {공개키 경로};
        ssl_certificate_key     {개인키 경로};
        ssl_protocols           TLSv1 TLSv1.1 TLSv1.2;

        root {웹 루트 경로};

        index index.html index.htm index.nginx-debian.html;

        location / {
                try_files $uri $uri/ =404;
        }

}

server {
    listen 80;
    server_name {도메인 주소 1} {도메인 주소 2};
    return 301 https://{도메인 주소};
}

출처: https://ndb796.tistory.com/341 [안경잡이개발자]Another Full Example
```

## NGINX 환경파일 공식문서
나는 Another Full Example 파일을 참고하였다. 
   
#### [Full Example Configuration](https://www.nginx.com/resources/wiki/start/topics/examples/full/#)
- [nginx.conf](https://www.nginx.com/resources/wiki/start/topics/examples/full/#nginx-conf) : 어플리케이션 기본 환경 설정
- [proxy.conf](https://www.nginx.com/resources/wiki/start/topics/examples/full/#proxy-conf) : 프록시 관련 환경 설정
- [fastcgi.conf](https://www.nginx.com/resources/wiki/start/topics/examples/full/#fastcgi-conf) : FastCGI 관련 환경 설정
- [mime.types](https://www.nginx.com/resources/wiki/start/topics/examples/full/#mime-types) : 파일 확장명과 MIME 타입 목록

#### [Another Full Example](https://www.nginx.com/resources/wiki/start/topics/examples/fullexample2/#)
- [nginx.conf](https://www.nginx.com/resources/wiki/start/topics/examples/fullexample2/#nginx-conf)
   
[참고] sites.conf : NGINX에 의해 서비스되는 가상 호스트 웹사이트 환경 설정. 도메인마다 파일을 분리해서 만들 것을 권장