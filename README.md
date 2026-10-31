# Codyssey Docker Practice

Docker와 GitHub를 이용한 개발 워크스테이션 실습 프로젝트입니다.

## 1. 프로젝트 소개

Docker의 기본 개념을 이해하고 Nginx 기반 웹 서버를 구축하며 이미지, 컨테이너, 포트, Bind Mount, Volume, Network의 사용법을 실습합니다.

## 2. 실습 환경

- OS: Windows
- Terminal: PowerShell
- Container Platform: Docker Desktop
- Base Image: Nginx Alpine
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
└── evidence/
    ├── assessment/
    └── assessment-backup.zip