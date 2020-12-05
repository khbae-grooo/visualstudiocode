nginx 가이드



## 설치 
```
$ brew update
$ brew install nginx
```

## listen 포트 & pid 확인방법 (TCP/UDP 세션 확인방법]
$ netstat -anv | grep LISTEN 


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

## nginx 포트 변경하기
~~~
$ vi /usr/local/etc/nginx/nginx.conf
# http server listen:8080 을 80으로 포트변경
# 저장 후 종료 (esc -> wq! -> ↩︎)
# http://locahost 접속하여 포트 확인
~~~

## nginx 포트를 다른포트로 연동하기 (proxy pass)

#### nginx.conf 파일 수정 및 재실행
#### nginx 설치 폴더의 conf폴더 내 nginx.conf 파일을 수정하여 server 부분에 아래의 코드만 남기고 제거합니다. 이렇게 해서 localhost:80으로 접근하면 자동으로 localhost:xxxx으로 연결되도록 합니다. 

```
$ vi /usr/local/etc/nginx/nginx.conf
```

#### nginx.conf 파일 내부 수정
~~~
server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_pass http://127.0.0.1:xxxx;  # 변경된 부분
            #root   html;
            #index  index.html index.htm;
        }

        #error_page  404              /404.html;
~~~
nginx.conf 파일을 wq! 명령어으로 저장후 종료
nginx 재시작 
```
$ nginx -s reload
```
웹 접속하면 http://localhost 가 http://locahost:xxxx로 재접속되는 것을 확인할 수 있다.
~~~
[출처 go웹앱을 NginX와 연동하기](https://streamls.tistory.com/101?category=830512)


## nginx 외부에서 접속하기, 포트포워딩

```
우리집의 인터넷 공급 업체는 SKB 브로드밴드인데, 80포트를 막아놓은 것 같다.

그래서 그냥 외부에서 nginx에 접속하려고 하면(=80포트가 열려있는지 확인하면) 안 되는 것을 겪을 수 있다.

이를 해결하기 위해서 맥 OS X(요세미티 이후)에서 포트포워딩을 하면 내부에선 80으로 설정하였지만 외부 인바운드 포트는 8080(예시, 아무렇게나 해도 됨)으로 설정해놓으면 외부에서 접속이 가능하다.

아래는 그 방법을 정리해놓은 것.

0. 기본적으로 공유기에서 외부포트 8080 -> 내부포트 80, 192.168.25.25(나의 경우)로 포트포워딩이 되어있어야 한다.

0-0. 요세미티 이전까지는 ipfw 를 사용하였는데 요세미티 이후엔 pf 를 사용한다.

1. /etc/pf.anchors/ 디렉토리에 com.test(파일 이름은 얼마든지 변경 가능, 애플에서는 기본적으로 com.apple 파일로 여러가지를 설정해놓은 것이 있기 때문에 비슷하게 com.test 라고 만들어 주었음)

$ sudo vi /etc/pf.anchors/com.test

2. 첫번째 라인에 아래의 한 줄짜리 설정정보를 입력하고 저장한다.

rdr pass on lo0 inet proto tcp from any to any port { 80 8080 } -> 127.0.0.1 port 80

뜻 : 외부에서 lo0(루프백 네트워크) 80 포트나 8080포트로 접속시 로컬호스트(127.0.0.1)의 80포트로 리다이렉팅한다.

3. /etc/pf.conf 파일을 편집한다. (아래의 내용을 추가한다.)
$ sudo vi /etc/pf.conf

rdr-anchor "com.test"
load anchor "com.test" from "/etc/pf.anchors/com.test"

4. 변경된 pf.conf를 적용한다.
$ sudo pfctl -evf /etc/pf.conf

5. you get singal 과 같은 사이트에서 포트 오픈을 확인한다.

나는 80은 닫혀있고 8080은 열려있다. (여기서 SKB에서 80을 막아놓았음을 확인할 수 있었다. 맥의 방화벽은 사용 안 하고 있었고, 공유기의 포트포워딩은 80, 8080 둘다 동일하게 적용이 되어 있었다. 찾아보니 KT에서는 알려진 포트 사용에 관대하다고 알려져있다.)

내 NO-IP 에서 제공받는 DDNS 도메인주소에 8080포트로 접속하면 내 nginx 루트 페이지가 열림을 확인하였다!
```
[출처] [nginx 외부에서 접속하기, 포트포워딩 링크](https://lhb0517.tistory.com/entry/OS-X-%EB%A7%A5-OS-X-nginx-%EC%99%B8%EB%B6%80%EC%97%90%EC%84%9C-%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0-%ED%8F%AC%ED%8A%B8%ED%8F%AC%EC%9B%8C%EB%94%A9?category=726837)

