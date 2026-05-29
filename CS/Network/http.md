# HTTP 요청/응답 구조

HTTP 통신은 클라이언트가 "요청(Request)"하고 서버가 "응답(Response)"을 돌려주는 구조

---

## 1. 요청(Request)의 구조

**요청 라인** — 메서드, 경로, HTTP 버전  
ex) `GET /api/user HTTP/1.1`

**헤더** — 요청에 대한 부가정보. "나는 누구고 어떤 형식의 데이터를 원해"  
ex) `Content-Type: application/json`, `Authorization: Bearer token123`

**바디** — 실제로 보내는 데이터. GET은 보통 바디가 없고 POST/PUT처럼 데이터를 보내는 요청에서 사용  
ex) `{"name": "홍길동", "age": 25}`

---

## 2. 응답(Response)의 구조

**상태 라인** — HTTP 버전, 상태코드, 상태메시지  
ex) `HTTP/1.1 200 OK`

**헤더** — 부가정보. `Content-Type`, `Set-Cookie`, `Cache-Control`

**바디** — 서버가 돌려주는 실제 데이터  
ex) HTML, JSON

---

payload는 내가 서버에 보낸 데이터 (GET 제외)  
header에는 응답/요청의 헤더 정보가 담겨있음  
response는 서버가 준 데이터가 담겨있다.

---

## HTTP 메서드

- GET — 데이터 조회용, 바디 없이 URL로만 요청
- POST — 생성용, 바디에 데이터를 담아서 보냄
- PUT — 전체 수정
- PATCH — 일부 수정
- DELETE — 삭제

---

## 상태코드

**2xx — 성공**
- 200 OK
- 201 Created — POST로 뭔가 새로 만들어졌을 때

**3xx — 리다이렉트**
- 301 — 영구 이동, 이 URL이 바꼈으니 새 URL로 가라는 뜻

**4xx — 클라이언트 잘못**
- 400 Bad Request — 요청 형식이 잘못됐을 때
- 401 Unauthorized — 인증 안 됐을 때 (로그인 안 됐을 때)
- 403 Forbidden — 인증은 있는데 권한이 없을 때
- 404 Not Found — 해당 리소스가 없을 때
- 409 Conflict — 중복 데이터 같은 충돌이 있을 때

**5xx — 서버 잘못**
- 500 Internal Server Error — 서버에서 뭔가 터졌을 때
- 502 Bad Gateway — 서버 앞단의 프록시가 뒷단 서버로부터 잘못된 응답을 받았을 때
- 503 Service Unavailable — 서버가 과부하이거나 점검 중일 때

---

## HTTP 버전 차이

**HTTP/1.1**  
요청 하나하나 보내고 응답 올 때까지 다음 요청을 못 보낸다.  
그래서 브라우저가 연결을 6개씩 열어서 우회했다.

**HTTP/2**  
하나의 연결에서 여러 요청을 주고 받는다. (멀티플렉싱)  
다만, TCP 패킷 하나가 유실되면 나머지도 같이 멈추는 문제가 있다.

**HTTP/3**  
TCP 대신 UDP 기반으로 바꿔서 패킷 유실이 다른 요청에 영향을 주지 않는다.  
연결 수립과 암호화도 한번에 처리해서 최초 접속도 빨라졌다.

★ HTTP/1.1, HTTP/2 → TCP 방식으로 전달  
★ HTTP/3 → UDP 방식으로 전달
