# Docker 실습 프로젝트

## 1. 프로젝트 개요

본 프로젝트는 Docker의 기본 개념과 컨테이너 실행 및 관리 방법을 익히고, Nginx 웹 서버 구축, 데이터 마운트 및 영속성 검증을 수행한 실습 프로젝트다.

Windows PowerShell 환경에서 Docker Desktop을 활용하여 이미지와 컨테이너를 관리하고, 웹 서비스 실행 및 포트 연결을 확인했다. 또한 Bind Mount와 Docker Volume을 이용해 호스트와 컨테이너 간 데이터 연결 및 데이터 유지 기능을 실습했다.

- **저장소:** https://github.com/SeoyoungShim/codyssey
- **운영체제:** Windows
- **터미널:** PowerShell
- **컨테이너 환경:** Docker Desktop (Linux containers)
- **주요 이미지:** Nginx, Alpine Linux, hello-world, Ubuntu

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
│   ├── assessment/
│   │   ├── bind-mount-final.txt
│   │   ├── collection-time.txt
│   │   ├── container-status-final.txt
│   │   ├── docker-build.txt
│   │   ├── docker-cleanup.txt
│   │   ├── docker-container-list.txt
│   │   ├── docker-containers.txt
│   │   ├── docker-image-list.txt
│   │   ├── docker-images.txt
│   │   ├── docker-info-final.txt
│   │   ├── docker-info.txt
│   │   ├── docker-network-inspect.txt
│   │   ├── docker-network-list.txt
│   │   ├── docker-port.txt
│   │   ├── docker-version.txt
│   │   ├── docker-volume-inspect.txt
│   │   ├── docker-volume-list.txt
│   │   ├── docker-volumes.txt
│   │   ├── file-management-final.txt
│   │   ├── file-operations.txt
│   │   ├── git-history.txt
│   │   ├── git-remote.txt
│   │   ├── git-status.txt
│   │   ├── hello-world.txt
│   │   ├── http-test-final.txt
│   │   ├── images-final.txt
│   │   ├── my-web-mount-final.txt
│   │   ├── permissions.txt
│   │   ├── port-test.txt
│   │   ├── ubuntu-container.txT
│   │   ├── volume-final.txt
│   │   ├── volume-persistence.txt
│   │   ├── web-server-response.txt
│   │   └── file-ops-demo/
│   ├── assessment-backup.zip
│   ├── bind-mount-browser.png
│   ├── bind-mount-ps.txt
│   ├── docker-browser.png
│   ├── docker-images.txt
│   ├── docker-ps.txt
│   ├── docker-version.txt
│   ├── docker-volume.txt
│   ├── web-8080-final.png
│   └── web-8081-final.png
└── logs/
    └── container.log
```
## 3. Docker 기본 개념

### 3.1 이미지(Image)와 컨테이너(Container)

| 구분 | 이미지(Image) | 컨테이너(Container) |
|---|---|---|
| 의미 | 실행 환경을 구성하는 템플릿 | 이미지를 기반으로 실행되는 인스턴스 |
| 역할 | 프로그램과 실행 환경 제공 | 실제 프로세스 실행 |
| 상태 | 이미지 자체는 실행 상태가 아님 | 생성, 실행, 정지 상태를 가짐 |
| 저장 | 읽기 전용 레이어로 구성 | 이미지 레이어 위에 쓰기 가능 계층 추가 |

이미지는 프로그램 실행을 위한 설계도이며, 컨테이너는 해당 설계도를 바탕으로 실행한 환경이다.

일반적인 실행 흐름은 다음과 같다.

1. Dockerfile 작성
2. `docker build` 명령으로 이미지 생성
3. `docker run` 명령으로 컨테이너 생성 및 실행
4. `docker stop` 명령으로 컨테이너 정지
5. `docker rm` 명령으로 컨테이너 삭제

컨테이너 내부에 저장한 데이터는 컨테이너 삭제 시 사라질 수 있다. 지속적으로 보존해야 하는 데이터는 Bind Mount 또는 Docker Volume을 활용할 수 있다.

### 3.2 Dockerfile

본 프로젝트에서 사용한 Dockerfile은 다음과 같다.

```dockerfile
FROM nginx:alpine

COPY site/index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

- `FROM`: 기반 이미지 지정
- `COPY`: 호스트의 파일을 이미지 내부로 복사
- `EXPOSE`: 컨테이너가 사용할 포트 정보 표시

Dockerfile을 기반으로 `my-nginx-site:1.0` 이미지를 생성했다.

## 4. Docker 주요 실습

### 4.1 Docker 환경 확인

Docker 설치 및 실행 환경을 확인했다.

```powershell
docker --version
docker info
```

### 4.2 이미지 및 컨테이너 관리

Docker 이미지를 확인하고 컨테이너를 실행·조회하는 명령어를 실습했다.

```powershell
docker images
docker ps
docker ps -a
docker run hello-world
```

주요 실습 내용은 다음과 같다.

- 이미지 목록 확인
- 실행 중인 컨테이너 확인
- 전체 컨테이너 확인
- hello-world 이미지 실행 결과 확인

### 4.3 Nginx 웹 서버 구축

Dockerfile을 이용하여 Nginx 기반 이미지를 빌드하고 웹 서버 컨테이너를 실행했다.

```powershell
docker build -t my-nginx-site:1.0 .
docker run -d --name my-web -p 8080:80 my-nginx-site:1.0
```

호스트의 8080 포트와 컨테이너의 80 포트를 연결하여 웹 서버에 접근할 수 있도록 구성했다.

### 4.4 포트 포워딩 검증

컨테이너의 웹 서버에 호스트에서 접근할 수 있는지 확인했다.

```powershell
docker port my-web
```

웹 브라우저에서 다음 주소에 접속했다.

- http://localhost:8080
- http://localhost:8081

두 주소 모두 HTTP 상태 코드 200을 반환하는 것을 확인했다.

### 4.5 Bind Mount

Bind Mount는 호스트의 특정 디렉터리를 컨테이너 내부 디렉터리와 연결하는 기능이다.

본 프로젝트에서는 `mount-test` 디렉터리를 Nginx 컨테이너의 웹 루트 디렉터리와 연결했다.

```powershell
$mountPath = (Resolve-Path ".\mount-test").Path

docker run -d --name mount-test-web -p 8081:80 --mount "type=bind,source=$mountPath,target=/usr/share/nginx/html" nginx:alpine
```

호스트의 `mount-test` 디렉터리에 있는 파일을 컨테이너의 웹 루트 디렉터리와 연결하여 호스트 파일 변경 사항이 컨테이너에 반영되는 구조를 확인했다.

### 4.6 Docker Volume

Docker Volume은 컨테이너의 생명주기와 별도로 데이터를 보관할 수 있는 저장 공간이다.

본 프로젝트에서는 `codyssey-data` Volume을 사용하여 데이터 저장과 유지 여부를 확인했다.

주요 확인 항목은 다음과 같다.

- Volume 생성 및 목록 확인
- Volume 상세 정보 확인
- Volume 내부 파일 확인
- 컨테이너 실행 환경과 별도로 데이터가 유지되는지 확인

### 4.7 Docker Network

Docker 기본 네트워크의 종류와 상세 설정을 확인했다.

```powershell
docker network ls
docker network inspect bridge
```

기본 네트워크의 이름, 드라이버, 서브넷, 게이트웨이 및 연결된 컨테이너 정보를 확인했다.

기록된 `bridge` 네트워크 상세 정보에서는 `my-web` 컨테이너가 연결되어 있으며, 컨테이너 IP는 `172.17.0.2`로 확인되었다.

## 5. Linux 파일 시스템 및 권한 실습

### 5.1 Ubuntu 컨테이너 실습

Ubuntu 컨테이너에서 수행한 실습 기록을 보관했다.

- 현재 작업 디렉터리 확인
- 디렉터리 목록 확인
- 운영체제 정보 확인
- 파일 생성 및 내용 확인
- 파일 권한 확인 및 변경
- 파일 상세 정보 확인
- 파일 이름 변경 및 삭제

실습에 사용한 주요 명령어는 다음과 같다.

```bash
docker run -it --rm ubuntu bash
pwd
ls
cat /etc/os-release
echo Hello, Ubuntu Docker! > hello.txt
cat hello.txt
ls -l hello.txt
chmod 600 hello.txt
stat hello.txt
mv hello.txt ubuntu-note.txt
rm ubuntu-note.txt
exit
```

### 5.2 파일 권한 확인

Linux 파일 권한을 확인하고 변경하는 실습을 수행했다.

기록된 권한 정보는 다음과 같다.

```text
-rw-r-----    1 root root 0 Oct 30 09:46 /tmp/permission-test
640 -rw-r----- /tmp/permission-test
```

권한 값 `640`은 소유자에게 읽기·쓰기 권한, 그룹에는 읽기 권한, 그 외 사용자에게는 권한이 없음을 의미한다.

### 5.3 파일 관리 실습

PowerShell을 이용하여 디렉터리 및 파일을 관리했다.

1. 디렉터리 생성
2. 파일 생성
3. 파일 내용 작성
4. 파일 내용 읽기
5. 파일 이름 변경
6. 파일 삭제
7. 최종 디렉터리 상태 확인

각 작업의 결과는 `file-management-final.txt`에 기록했다.

## 6. 최종 실습 검증 결과

2026년 11월 2일 Docker Desktop 환경에서 기존 컨테이너와 데이터의 정상 작동 여부를 재검증했다.

### 6.1 웹 서버 및 포트 연결

| 컨테이너 | 호스트 포트 | 컨테이너 포트 | 검증 결과 |
|---|---:|---:|---|
| my-web | 8080 | 80 | HTTP 200 |
| mount-test-web | 8081 | 80 | HTTP 200 |

### 6.2 데이터 마운트

- `my-web`: 별도 마운트 없이 실행
- `mount-test-web`: 호스트의 `mount-test` 폴더를 컨테이너 웹 루트에 Bind Mount
- `codyssey-data`: Docker Volume 생성 및 데이터 유지 확인

### 6.3 데이터 영속성 검증

Docker Volume에 저장한 `assessment-test.txt`와 `test.txt` 파일이 컨테이너 실행 환경이 바뀐 뒤에도 유지되는 것을 확인했다.

검증 메시지:

```text
Volume persistence verified!
```

### 6.4 네트워크 및 파일 권한

- Docker 기본 네트워크 목록 및 `bridge` 상세 정보 확인
- `my-web` 컨테이너의 네트워크 연결 정보 확인
- Linux 파일 권한 `640` 확인

### 6.5 파일 관리

파일 생성, 읽기, 이름 변경, 삭제 작업이 정상적으로 수행되었음을 확인했다.

## 7. 프로젝트 실행 방법

프로젝트를 내려받은 후 PowerShell에서 프로젝트 폴더로 이동한다.

```powershell
git clone https://github.com/SeoyoungShim/codyssey.git
cd codyssey
```

Docker Desktop이 실행 중인 상태에서 다음 명령어를 실행한다.

```powershell
docker build -t my-nginx-site:1.0 .
docker run -d --name my-web -p 8080:80 my-nginx-site:1.0
```

> 참고: 이미 `my-web`이라는 이름의 컨테이너가 존재한다면 기존 컨테이너를 정리한 후 실행해야 한다.

웹 브라우저에서 다음 주소로 접속한다.

```text
http://localhost:8080
```

컨테이너 실행 상태는 다음 명령어로 확인할 수 있다.

```powershell
docker ps
```

실습이 끝난 뒤 컨테이너를 정리하려면 다음 명령어를 사용한다.

```powershell
docker stop my-web
docker rm my-web
```

## 8. 프로젝트 결론

이번 실습을 통해 Docker의 기본 구조와 이미지 및 컨테이너 관리 방법을 익혔다. 또한 Nginx 웹 서버를 직접 구축하고 포트 포워딩을 통해 서비스 접근을 확인했다.

Bind Mount와 Docker Volume을 활용하여 데이터 연결 방식과 영속성의 차이를 확인했으며, Linux 파일 시스템과 권한 관리, Docker Network 확인 및 GitHub를 통한 실습 자료 관리도 수행했다.

실습 과정과 검증 결과는 `evidence` 폴더에 기록하여 프로젝트의 수행 과정을 확인할 수 있도록 정리했다.

## 9. 평가 항목별 추가 설명

### 9.1 Docker 네트워크와 포트 포워딩
Docker 컨테이너는 독립된 네트워크 환경에서 실행된다. `-p 8080:80`은 호스트의 8080번 포트와 컨테이너의 80번 포트를 연결한다. 네트워크 정보는 `docker network ls`, `docker network inspect bridge` 명령으로 확인할 수 있다.

### 9.2 절대 경로와 상대 경로
절대 경로는 파일 시스템의 기준 위치부터 전체 경로를 표시한다. 상대 경로는 현재 작업 디렉터리를 기준으로 표시한다. Bind Mount에서는 호스트 경로와 컨테이너 내부 경로를 각각 지정해야 한다. PowerShell의 `Resolve-Path .\mount-test`로 실제 경로를 확인할 수 있다.

### 9.3 Linux 파일 권한
Linux 권한은 소유자, 그룹, 기타 사용자 순서로 표시한다. 읽기(r)는 4, 쓰기(w)는 2, 실행(x)은 1이다. 따라서 640은 소유자에게 읽기와 쓰기, 그룹에 읽기 권한을 부여하고 기타 사용자에게는 권한을 부여하지 않는다. 실제 확인 결과는 `evidence/assessment/permissions.txt`에 기록했다.

### 9.4 포트 충돌 진단
포트 충돌이 발생하면 먼저 `docker ps`로 컨테이너의 포트 매핑을 확인한다. 이후 Windows에서 `netstat -ano | findstr :8080`으로 포트 사용 프로세스를 확인하고, `Get-Process -Id PID`로 프로세스를 식별한다. 필요하면 다른 포트를 사용한다.

### 9.5 데이터 영속성과 백업
Bind Mount는 호스트의 지정 경로와 컨테이너를 연결한다. Docker Volume은 Docker가 관리하는 저장 공간을 사용한다. 컨테이너 삭제 후에도 Volume의 데이터는 유지될 수 있으므로 중요한 데이터는 별도로 백업해야 한다. Volume은 `docker volume ls`, `docker volume inspect`로 확인한다.

### 9.6 문제 해결 과정
문제 해결은 문제 현상 파악, 원인 가설 설정, 명령어를 통한 확인, 조치, 재검증 순서로 수행한다. 이번 실습에서는 HTTP 응답 본문의 한글 표시 문제를 확인하기 위해 원본 HTML을 UTF-8로 읽고, 웹 서버의 응답 상태를 별도로 확인했다. 8080번과 8081번 포트에서 HTTP 200 응답을 확인했다.

### 9.7 평가 증거자료
실제 실습 결과는 `evidence/assessment/`에 저장했다. Docker 버전과 환경, 이미지 및 컨테이너, 네트워크, 포트 접속, Bind Mount, Volume 영속성, Linux 권한, 파일 관리 및 Git 이력 자료를 확인할 수 있다.
## 10. 추가 평가 증거자료

다음 자료는 파일 관리와 Linux 파일 권한 실습을 직접 수행하고 기록한 결과다.

| 증거 파일 | 주요 내용 |
|---|---|
| `evidence/assessment/final-file-operations.txt` | 임시 파일 생성, 이동, 삭제 및 삭제 여부 확인 |
| `evidence/assessment/final-chmod-verification.txt` | Docker Alpine 컨테이너에서 chmod 640 실행 및 ls -l, stat 결과 확인 |

파일 삭제 검증 결과 `False`는 삭제 대상 파일이 더 이상 존재하지 않음을 의미한다. 권한 검증 결과 `640`은 소유자에게 읽기·쓰기, 그룹에는 읽기, 기타 사용자에게는 권한이 없음을 의미한다.
