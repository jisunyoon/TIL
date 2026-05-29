# XSS란?

공격자가 입력한 문자열이 다른 사용자의 브라우저에서 코드로 실행되는 공격  
= 데이터야 할 게 코드가 되어버린다.  
(사용자 입력은 화면에 글자로 보여야 하는데 `<script>`나 `<onError>` 같은 형태로 HTML에 박혀서 실행된다.)

---

## stored / reflected / DOM XSS의 차이

### 1. Stored XSS (저장형)

악성 스크립트가 서버 DB에 저장됐다가, 다른 사용자가 그 페이지를 열 때마다 실행

```
[공격자]   댓글에 <script>...</script> 입력
[서버 DB]  그대로 저장
[피해자 A/B/C] 게시글 조회 → 스크립트 실행
```

공격자가 한 번 심어두면 그 글을 보는 사용자가 자동으로 당함  
위의 세 개 중 가장 위험한 유형  
"여러 사람들이 보는 저장공간이 주 표적"

---

### 2. Reflected XSS (반사형)

악성 스크립트가 URL이나 요청 파라미터에 담겨서 서버로 갔다가 응답에 그대로 "반사"되어 돌아와서 실행된다. (서버 저장은 안 됨)

```
[공격자]  URL 만듦: https://shop.com/search?q=<script>document.location='attacker.com'</script>
[피해자]  이 링크를 클릭 (피싱 메일, 메신저 링크 등)
[서버]    검색 결과 페이지 응답 "검색어 <script>...</script>에 대한 결과입니다."
[피해자 브라우저] HTML 렌더링 하면서 스크립트 실행
```

Stored와 달리 피해자가 그 특정 URL을 클릭하게 유도해야 함

---

### 3. DOM-based XSS

앞에 애들과 달리 서버를 아예 거치지 않고 클라이언트 JS에서만 발생  
앞에 두 유형은 서버가 응답 HTML에 악성 문자열을 박아서 보내주는 게 문제였다.  
반면에 DOM XSS는 서버가 보낸 HTML은 문제 없는데, 클라이언트 JS가 URL이나 입력값을 받아서 DOM에 직접 박을 때 발생

```
[공격자]  https://site.com/profile#<img src=x onerror=alert(1)>
[피해자]  URL 접속
[서버]    깨끗한 HTML 응답 (서버는 # 뒤 내용을 못 봄)
[클라이언트 JS]  location.hash 읽음 → innerHTML에 박음
[브라우저] DOM에 <img onerror=...> 생성 → 실행
```

서버는 공격의 존재를 모른다. 로그에도 흔적 없음

---

## 해결 방법

- `innerHTML` 보다 `textContent` 사용 (글자로만 처리)
- DOMPurify 라이브러리 사용 = 라이브러리가 알아서 `<script>`, `<a>` 태그 등 악성인지 아닌지를 걸러내줌
