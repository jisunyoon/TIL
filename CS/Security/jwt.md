# JWT 구조와 동작

JWT는 `[header].[payload].[signature]` 로 데이터의 정보가 나뉘어져 있다.

header와 payload는 base64로 인코딩 되어있어서 누구나 jwt.io 같은 사이트로 디코딩해서 JSON으로 정보를 볼 수 있다.  
대신 signature가 비밀키로 만들어져서 위조를 막음

---

## 만료 처리

JWT의 만료 처리는 서버에서 정해주는데 access / refresh 토큰에 각각 다른 기간을 줌

- **access token** — 몇 분 단위로 만료. 만료되면 refresh로 다시 요청
- **refresh token** — 주 단위로 DB에 저장. 만료되면 다시 로그인해야 함

---

## 토큰을 두 개로 나누는 이유

- access token → 보안 (짧게 유지해서 탈취돼도 피해 최소화)
- refresh token → 자동 갱신 (매번 로그인 안 해도 되게)

---

## 저장 위치

- access token → 메모리나 localStorage
- refresh token → HttpOnly 쿠키로 저장하는 것을 추천 (HttpOnly 쿠키는 JS에서 접근 불가 → XSS로 탈취 안 됨)

payload나 header에는 민감하고 중요한 정보를 넣으면 안 됨
