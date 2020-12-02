
# 초기화

## **visual sutdio code with go**

### go 설치 후,
bin : go 에서 사용하는 명령어 들이 저장
pkg : go get 명령어로 다운 받은 패키지들이 저장됩니다.
src : 실제 go 파일 소스들이 있어야 합니다.
3개의 폴더 생성

### 환경 설정
GOPATH
GOROOT



## vscode에서 go 플러그인을 설치합니다.
install all with go-outline
golint도 설치

## VS Code를 실행후 메뉴 » Code » Preference » User Settings를 클릭하자. 단축키는 ⌘ + ,
에티터 화면에서 default Setting과 setting.json이 보인다.

vscode go 확장 옵션 중 inferGopath 라는 옵션이 있다. 이 옵션을 키면 Gopath 값을 현재 workspace 로 덮어쓰는 기능이 있다. 그래서 아래와 같이 .vscode/settings.json 을 구성하게 되었다.

## settings.json
{
    "go.gopath": "${workspaceFolder}", // gopath 설정을 vscode 의 workspace 로 한다. 
    "go.inferGopath": true, // 이 옵션을 켜야 위의 설정이 먹히는 것 같다.
    "terminal.integrated.env.windows": {
        "GOPATH": "${workspaceFolder}" // windows 에서 vscode 프로그램 내 터미널을 GOPATH 환경 변수 변경
    },
    "terminal.integrated.env.osx": {
        "GOPATH": "${workspaceFolder}" // osx 에서 vscode 프로그램 내 터미널을 GOPATH 환경 변수 변경
    },
}

another settings
## settings.json
{
    "go.buildOnSave": true,
    "go.lintOnSave": true,
    "go.vetOnSave": true,
    "go.buildTags": "",
    "go.buildFlags": [],
    "go.lintFlags": [],
    "go.vetFlags": [],
    "go.coverOnSave": false,
    "go.useCodeSnippetsOnFunctionSuggest": false,
    "go.formatOnSave": true,
    // goreturns 은 goimports(자동 임포트), gofmt(자동 포맷팅)를 사용하고 리턴코드도 자동으로 채워준다.
    "go.formatTool": "goreturns",
    "go.gocodeAutoBuild": false,
    // 맥,리눅스 기준
    //"go.goroot": "/usr/local/go",
    //"go.gopath": "/home/nam/projects/go"
    // 윈도우 기준
    "go.goroot": "c:\\go",
    "go.gopath": "d:\\projects\\Go"
}
출처: https://hcnam.tistory.com/6 [segmentation fault (core dumped)]

# launch을 사용하여 디버깅 구성
F5 를 누르면 .vscode/launch.json 를 작성하라고 나온다. 물론 직접 만들어서 작성해도 된다. dlv debug 를 위한 세팅은 아래 처럼 해놨다.
## launch.json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [ 
        {
            "name": "Launch",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "${fileDirname}",
            "env": {},
            "args": []
        }
    ]
}


대신 패키지를 설치하기 위해서는 꼭 vscode 프로그램 내 터미널을 사용해야 한다. 

그러나 이렇게 하면 독립적인 환경을 구축할 수는 있는데, workspace 를 git 그대로 설정하게 되면 패키지들도 git에 포함될 수 있다. gitignore 를 잘 설정해야 할 것 같다. 

그리고 패키지를 이렇게 수동으로 설정해야 하는지 잘모르겠다. python 처럼 requirements.txt 파일로 pip 로 빠르게 설지하면 좋을 것 같은데, 아직 이런 방법은 잘 모르겠다. 

## VS Code 환경설정 동기화로 편리하게 사용하자
[VS Code 환경설정 동기화로 편리하게 사용하자](https://kimyejin.tistory.com/40)





파일 이름	용도
tasks.vs.json	사용자 지정 빌드 명령 및 컴파일러 스위치와 임의(빌드와 관련되지 않은) 작업을 지정합니다.
솔루션 탐색기 의 오른쪽 클릭 메뉴 항목 작업 구성 을 통해 액세스합니다.
launch.vs.json	디버깅을 위한 명령줄 인수를 지정합니다.
솔루션 탐색기 의 오른쪽 클릭 메뉴 항목 디버그 및 시작 설정 을 통해 액세스합니다.
