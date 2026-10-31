# Codyssey Docker Practice

Docker와 GitHub를 활용하여 개발 워크스테이션을 구축하고 컨테이너 기반 웹 서버를 운영하는 실습 프로젝트입니다.

## 1. 프로젝트 소개

Docker의 기본 개념과 사용법을 익히고 Nginx 기반 웹 서버를 구축했습니다. 이미지 생성, 컨테이너 실행, 포트 연결, Bind Mount, Volume, Network 및 GitHub 연동을 실습했습니다.

## 2. 실습 환경

- OS: Windows
- Terminal: PowerShell
- Container Platform: Docker Desktop
- Base Image: Nginx Alpine
- Additional Image: Ubuntu
- Web Server Port: 80
- Host Port: 8080

## 3. 프로젝트 구조

```text
codyssey/
├── Dockerfile
├── README.md
├── site/
│   └── index.html
├── logs/
├── mount-test/
└── evidence/
    ├── assessment/
    └── assessment-backup.zip
```

## 4. 주요 실습 내용

### 4-1. Docker 기본 명령어
- Docker 버전 및 시스템 정보 확인
- 이미지 목록 및 컨테이너 목록 확인
- hello-world 이미지 실행

### 4-2. Dockerfile 작성 및 이미지 빌드
- Nginx Alpine 기반 Dockerfile 작성
- 사용자 HTML 파일을 컨테이너 내부 웹 경로에 복사
- my-nginx-site:1.0 이미지 생성

### 4-3. 컨테이너 실행 및 포트 연결
- Nginx 컨테이너 실행
- 호스트 8080 포트와 컨테이너 80 포트 연결
- 브라우저 및 HTTP 요청으로 웹 서버 응답 확인

### 4-4. Bind Mount
- 호스트 폴더와 컨테이너 내부 폴더 연결
- 호스트 파일 변경이 컨테이너에 반영되는 과정 확인

### 4-5. Docker Volume
- codyssey-data 볼륨 생성
- 컨테이너에서 데이터 저장
- 컨테이너 삭제 후에도 데이터가 유지되는지 확인

### 4-6. Docker Network
- Docker 네트워크 목록 및 상세 정보 확인
- 컨테이너의 네트워크 연결 정보 확인

### 4-7. Ubuntu 컨테이너 실습
- Ubuntu 컨테이너 실행
- 디렉터리 탐색 및 운영체제 정보 확인
- 파일 생성, 읽기, 이름 변경 및 삭제
- chmod를 이용한 파일 권한 변경
- ls -l 및 stat으로 파일 정보 확인

## 5. Dockerfile

```dockerfile
FROM nginx:alpine

COPY site/index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

## 6. 이미지 빌드

```powershell
docker build -t my-nginx-site:1.0 .
```

## 7. 컨테이너 실행

```powershell
docker run -d --name my-web -p 8080:80 my-nginx-site:1.0
```

## 8. 웹 서버 접속

브라우저에서 아래 주소로 접속합니다.

http://localhost:8080

## 9. 문제 해결 기록

### 사례 1. Git에서 새 증거자료가 등록되지 않는 문제
- 상황: Ubuntu 실습 기록 파일이 Git에서 Untracked 상태로 표시됨
- 원인: 실제 파일 확장자가 `txT`로 저장되어 명령어에 입력한 파일명과 일치하지 않음
- 해결: 실제 파일명을 확인하고 `git add -f` 명령어로 등록한 후 커밋 및 푸시 진행

### 사례 2. Ubuntu 이미지가 로컬에 없는 경우
- 상황: Ubuntu 컨테이너 실행 시 로컬에 이미지가 존재하지 않음
- 해결: Docker가 Ubuntu 이미지를 자동으로 내려받는 과정을 확인하고 컨테이너 실행 완료

## 10. 증거자료

실습 과정과 결과는 evidence 폴더에 저장했습니다.

주요 자료:
- Docker 버전 및 시스템 정보
- 이미지 빌드 및 컨테이너 실행 기록
- 포트 연결 및 웹 서버 응답 기록
- Docker Volume 및 Network 확인 자료
- Bind Mount 실습 화면 및 기록
- Ubuntu 컨테이너 실습 기록
- assessment-backup.zip 백업 파일

## 11. GitHub 저장소

https://github.com/SeoyoungShim/codyssey