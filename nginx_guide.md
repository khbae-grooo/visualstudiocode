nginx 가이드



설치 

$ brew update
$ brew install nginx

## install 후 nginx, openssl@1.1, pcre 설치됨

## nginx 시작
$ nginx
## nginx 정지
$ nginx -s stop
## nginx 재시작
$ nginx -s reload

## nginx 웹 포트 8080
## 계정권한이 root가 아닌 경우 1024이하 포트를 사용할 수 없다
## $ sudo nginx 를 실행해서 포트를 변경할 수 있다

