# Mac 터미널에서 명령어 종류

## listen 포트 & pid 확인방법 (TCP/UDP 세션 확인방법]
$ netstat -anv | grep LISTEN 

# 실행중 프로세스 점유율 확인(좀비 프로세스 찾기)  https://seulcode.tistory.com/545
$ ps -aux | grep process 

# 실행중 프로세스 및 포트 찾기 명령어  [UID PID PPID]
$ps -ef | grep process 

# 오픈된 포트 확인
$ netstat -nat | grep LISTEN

# 포트사용중인 프로세스 찾기
$ lsof -n -i4tcp:8000

# 프로세스 죽이기
$ kill -9 pid
