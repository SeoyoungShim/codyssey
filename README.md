# Docker 실습 프로젝트

## 1. 프로젝트 개요

본 프로젝트는 Docker의 기본 구조와 이미지·컨테이너 관리, 웹 서비스 배포, 포트 포워딩, 데이터 마운트 및 영속성을 실습하고 그 결과를 기록한 프로젝트다.

Windows PowerShell 환경에서 Docker를 사용하여 Nginx 웹 서버를 실행하고, 호스트 파일과 컨테이너를 연결하는 Bind Mount 및 Docker Volume의 데이터 유지 기능을 검증했다.

- 저장소: https://github.com/SeoyoungShim/codyssey
- 운영체제: Windows
- 터미널: PowerShell
- 컨테이너 환경: Docker Desktop (Linux containers)
- 주요 이미지: Nginx, Alpine Linux, hello-world

## 2. 프로젝트 디렉터리 구조

```text
codyssey/
├── Dockerfile
├── README.md
├── site/
│   └── index.html
├── mount-test/
│   └── index.html
├── evidence/
│   ├── bind-mount-browser.png
│   ├── bind-mount-ps.txt
│   ├── docker-browser.png
│   ├── docker-images.txt
│   ├── docker-ps.txt
│   ├── docker-version.txt
│   ├── docker-volume.txt
│   └── assessment/
│       ├── file-ops-demo/
│       ├── collection-time.txt
│       ├── docker-build.txt
│       ├── docker-cleanup.txt
│       ├── docker-containers.txt
│       ├── docker-images.txt
│       ├── docker-info.txt
│       ├── docker-version.txt
│       ├── docker-volumes.txt
│       ├── file-operations.txt
│       ├── git-history.txt
│       ├── git-remote.txt
│       ├── git-status.txt
│       ├── hello-world.txt
│       ├── permissions.txt
│       ├── port-test.txt
│       └── volume-persistence.txt
└── logs/
    └── container.log
## 3. Docker 기본 개념

### 3.1 이미지와 컨테이너의 차이

| 구분 | 이미지(Image) | 컨테이너(Container) |
|---|---|---|
| 의미 | 실행 환경을 구성하는 템플릿 | 이미지를 기반으로 실행되는 인스턴스 |
| 역할 | 프로그램과 실행 환경 제공 | 실제 프로세스 실행 |
| 상태 | 이미지 자체는 실행 상태가 아님 | 생성, 실행, 정지 상태를 가짐 |
| 저장 | 읽기 전용 레이어로 구성 | 이미지 레이어 위에 쓰기 가능 계층 추가 |

이미지는 프로그램 실행을 위한 설계도이고, 컨테이너는 해당 설계도를 바탕으로 실제 실행한 환경이다.

일반적인 실행 흐름은 다음과 같다.

1. Dockerfile 작성
2. `docker build` 명령으로 이미지 생성
3. `docker run` 명령으로 컨테이너 생성 및 실행
4. `docker stop` 명령으로 컨테이너 정지
5. `docker rm` 명령으로 컨테이너 삭제

컨테이너 내부에 저장한 데이터는 컨테이너 삭제 시 사라질 수 있다. 지속적으로 보존해야 하는 데이터는 Bind Mount 또는 Docker Volume에 저장해야 한다.

### 3.2 Dockerfile

본 프로젝트의 Dockerfile은 다음과 같다.

```dockerfile
FROM nginx:alpine

COPY site/index.html /usr/share/nginx/html/index.html

EXPOSE 80
## 6. 최종 실습 검증 결과

2026년 11월 2일, Docker Desktop 환경에서 기존 컨테이너와 데이터의 정상 작동 여부를 재검증했다.

### 6.1 웹 서버 및 포트 연결

| 컨테이너 | 호스트 포트 | 컨테이너 포트 | 검증 결과 |
|---|---:|---:|---|
| my-web | 8080 | 80 | HTTP 200 |
| mount-test-web | 8081 | 80 | HTTP 200 |

### 6.2 데이터 마운트

- my-web: 별도 마운트 없이 실행
- mount-test-web: 호스트의 mount-test 폴더를 컨테이너 웹 루트에 Bind Mount
- codyssey-data: Docker Volume 생성 및 데이터 유지 확인

### 6.3 데이터 영속성 검증

Docker Volume에 저장한 assessment-test.txt와 test.txt 파일이 컨테이너 실행 환경이 바뀐 뒤에도 유지되는 것을 확인했다.

검증 메시지: `Volume persistence verified!`

### 6.4 최종 증거 자료

세부 실행 결과는 evidence/assessment/ 폴더에 저장했다.
