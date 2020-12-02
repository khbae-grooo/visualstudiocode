디펜던시 파일 생성

디펜던시 파일을 생성해 주어야 Heroku에서 앱으로 인식을 합니다. govendor나 godep 등 여러 프로그램들이 있지만, 기본 설치되어있는 go module 프로그램으로 충분합니다. (사실 위 두 프로그램으로 시도해 보다가 실패함..)

<1> go mod init        //go.mod파일 생성

<2> go build

<3> go mod vendor   //vendor폴더에 디펜던시 파일 다운받음
