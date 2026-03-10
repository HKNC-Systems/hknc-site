# HKNC Nginx + SSI Boilerplate

```
Docker + Nginx 기반의 **SSI(Server Side Includes) 지원 정적 사이트 개발 환경**입니다.
기존 wordpress 결과물에서 resource 추출하여 shtml, html, js, css, png/jpg 등의 resource 생성하였습니다.
```
---

# 1. Prerequisites

프로젝트 실행 전 아래 항목을 확인하세요.

## Docker 설치 확인

```bash
docker --version
docker compose version
```

### Windows 사용자

1. **Docker Desktop 실행**
2. 아래 메시지가 표시되면 WSL 업데이트

```
WSL needs updating
```

CMD에서 실행

```bash
wsl --update
```

Docker Desktop이 **Running 상태**인지 트레이에서 확인합니다.

---

# 2. VSCode Extensions

## 필수

- Docker (Microsoft)

## 권장

- ESLint
- Prettier - Code formatter

## 선택

- NGINX Configuration
- nginx.conf syntax highlight
- Dev Containers
- Remote - Containers

---

# 3. 프로젝트 실행

프로젝트 루트에서 실행

```bash
docker compose up -d
```

다음 메시지가 나오면 정상 실행입니다.

```
✔ Container hknc-nginx started
```

다음 경고는 **에러가 아닙니다**

```
`version` is obsolete
```

---

# 4. 접속 테스트

브라우저에서 확인

```
http://localhost
```

---

# 5. SSI 사용 방법

`.shtml` 파일에서 아래 형식으로 include 사용

```html
<!--#include virtual="/assets/html/header_index.html" -->
```

파일 설명

```
/hknc/_includes/header.shtml
/hknc/_includes/footer.shtml
hknc_site/assets/html/head_partial.html: head 태그에서 title 제외한 공통 부분
hknc_site/assets/html/header_index.html: index 페이지에서 사용하는 header태그 공통 부분
hknc_site/assets/html/header_other.html: index 이외의 페이지에서 사용하는 header태그 공통 부분
hknc_site/assets/html/category.html: 일부 페이지에서 사용하는 카테고리 리스트
hknc_site/assets/html/footer_scripts.html: footer 위에서 script 호출하는 공통부분
hknc_site/assets/html/footer.html: 모든 페이지에서 사용하는 footer태그 공통부분
```

---

# 6. HTTPS 설정

SSL 인증서를 아래 경로에 배치합니다.

```
nginx/ssl/
```

필요 파일

```
cert.pem
key.pem
```

---

# 7. 프로젝트 구조

```
project-root
│
├─ nginx
│   └─ ssl
│       ├─ cert.pem
│       └─ key.pem
│
├─ nginx
│   └─ nginx.conf: docker 내의 nginx에서 적용될 설정파일
│
└─ hknc_site
    └─ (정적 사이트 파일)
```

| 폴더 | 설명 |
|-----|-----|
| nginx | Docker에서 사용하는 Nginx 설정 |
| hknc_site | 실제 배포되는 웹 프로젝트 |

---

# 8. Nginx 파일 탐색 우선순위

다음 URL 요청 시

```
http://localhost/about
```

Nginx는 아래 순서로 파일을 찾습니다.

```
/usr/share/nginx/html/hknc/about.shtml
/usr/share/nginx/html/hknc/about.html
/usr/share/nginx/html/hknc/about/index.shtml
/usr/share/nginx/html/hknc/about/index.html
```

---

# 9. Docker 명령어

## 컨테이너 상태 확인

```bash
docker ps
```

## 중지

```bash
docker compose down
```

## 재시작

```bash
docker compose restart
```

## 로그 확인

```bash
docker logs hknc-nginx
```

---

# 10. 컨테이너 내부 접속

```bash
docker exec -it hknc-nginx /bin/bash
```

또는

```bash
docker exec -it hknc-nginx /bin/sh
```

---

# 11. 컨테이너 내부 파일 확인

```bash
docker exec -it hknc-nginx ls -R /usr/share/nginx/html
```

---

# 12. 컨테이너 완전 초기화

볼륨 포함 삭제

```bash
docker compose down -v
```

다시 실행

```bash
docker compose up -d
```
