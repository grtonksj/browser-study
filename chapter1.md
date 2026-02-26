# 🌐 Web Browser Engineering: Chapter 1

> **웹페이지 다운로드 (Downloading Web Pages)**

---

# 🧭 Chapter 1 핵심 흐름

브라우저는 아래 과정을 반복합니다.

1. URL 해석
2. TCP 연결 (소켓)
3. HTTP 요청 전송
4. HTTP 응답 수신
5. HTML 추출

👉 이 장은 **"브라우저는 결국 텍스트를 주고받는 프로그램"**이라는 걸 이해하는 게 핵심입니다.

---

# 1.1 서버에 연결하기

* URL → (스킴, 호스트, 포트, 경로)로 분해
* 소켓(socket)을 생성해 서버와 TCP 연결
* 기본 포트

  * HTTP → 80
  * HTTPS → 443

👉 브라우저는 결국 "IP + 포트"에 연결하는 프로그램

---

# 1.2 정보 요청하기 (Request)

브라우저는 서버에 텍스트 기반 요청을 보냅니다.

```http
GET /index.html HTTP/1.1
Host: example.com
```

### 핵심 요소

* Method: GET
* Path: /index.html
* Version: HTTP/1.1
* Header: Host (필수)

👉 HTTP는 **문자열 프로토콜**이다

---

# 1.3 서버의 응답 (Response)

서버는 요청을 받고 응답을 돌려줍니다.

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>...</html>
```

### 구성

1. 상태라인 (Status Line)
2. 헤더 (Headers)
3. 바디 (Body)

👉 브라우저는 이 문자열을 파싱해서 화면을 만듦

---

# 1.4 파이썬으로 직접 요청하기

* 소켓에 직접 HTTP 요청 문자열 작성
* 서버 응답을 그대로 출력

👉 fetch / axios는 사실 이 과정을 추상화한 것

---

# 1.5 HTTP 포맷 규칙

HTTP는 엄격한 규칙이 있음

* 줄 끝은 반드시 `\r\n`
* 헤더와 바디 사이에는 빈 줄

```http
GET / HTTP/1.1\r\n
Host: example.com\r\n
\r\n
```

👉 이 규칙 깨지면 서버가 이해 못함

---

# 1.6 HTML 표시하기

* 응답에서 헤더 제거
* 바디만 추출

👉 아직은 단순 텍스트 출력
👉 렌더링은 다음 챕터

---

# 1.7 HTTPS (암호화 연결)

* TLS 사용
* 소켓을 암호화

👉 HTTPS = HTTP + TLS

---

# 1.8 추가 개념 (연습문제 기반 확장)

## 1) HTTP/1.1 헤더 확장

```http
Connection: close
User-Agent: my-browser
```

👉 브라우저는 다양한 헤더로 자신을 설명함

---

## 2) 파일 URL (file://)

* 서버 없이 로컬 파일 읽기

👉 브라우저는 네트워크 프로그램이면서 파일 뷰어

---

## 3) data 스킴

```text
data:text/html,Hello world
```

👉 URL 자체에 데이터를 포함

---

## 4) HTML 엔티티

```html
&lt;div&gt;
```

👉 실제 출력: `<div>`

---

## 5) view-source

```text
view-source:http://example.com
```

👉 렌더링 대신 원본 HTML 표시

---

## 6) Keep-Alive

* 연결을 재사용
* 매 요청마다 소켓 생성 X

👉 성능 핵심 요소

---

## 7) 리다이렉트

* 상태 코드: 301, 302
* Location 헤더로 이동

👉 브라우저는 자동으로 재요청

---

## 8) 캐싱

```http
Cache-Control: max-age=3600
```

👉 네트워크 요청을 줄임

---

## 9) 압축

```http
Accept-Encoding: gzip
Content-Encoding: gzip
```

👉 전송 데이터 크기 감소

---

# 🎯 핵심 요약

* URL을 스킴, 호스트, 포트, 경로로 파싱합니다.
* sockets과 ssl 라이브러리를 사용해 해당 호스트에 연결합니다.
* Host 헤더를 포함해 해당 호스트에 연결합니다.
* HTTP 응답을 상태 표시줄, 헤더, 바디로 분할합니다.
* 바디에서 태그가 아닌 텍스트를 출력한다.

👉 결국 **"문자열 통신 + 파싱"**

---

# 🚀 핵심 연습 문제

> ❗ 필수만 진행 (10~15분 내 토론 가능)

---

## 1. HTTP 요청 구조 이해

### 문제

브라우저 요청을 직접 설계해보세요.

```http
GET / HTTP/1.1
Host: example.com
Connection: close
```

👉 질문

* Host 헤더가 왜 필요할까?

---

## 2. 요청 → 응답 흐름

### 문제

브라우저가 페이지를 가져올 때 순서를 설명하세요.

👉 기대 답변

1. TCP 연결
2. HTTP 요청
3. HTTP 응답
4. HTML 수신

---

## 3. Keep-Alive 개념

### 문제

이미지 100개 페이지에서

* Connection: close
* Connection: keep-alive

차이를 설명하세요.

👉 핵심

* 연결 재사용 vs 매번 새 연결

---

## 4. 리다이렉트

### 문제

```http
HTTP/1.1 302 Found
Location: /login
```

브라우저는 어떻게 동작할까?

👉 핵심

* 새로운 요청을 다시 보냄

---

## 5. HTTPS 필요성

### 문제

HTTP 대신 HTTPS를 쓰는 이유는?

👉 핵심

* 데이터 암호화
* 중간자 공격 방지

---
