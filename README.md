# TIL — CS 학습 기록

CS 이론·코딩테스트·프로젝트를 정리하는 저장소입니다.

---

## Phase 1 ✅

### CS/네트워크 & 보안
- 인터넷 동작 원리 / DNS / TCP vs UDP
- HTTP 요청·응답 구조 / 메서드·상태코드 / HTTP 버전(1.1·2·3)
- HTTPS + TLS 핸드셰이크
- 쿠키 vs 세션 vs 토큰
- CORS 원리와 해결법
- 캐시 헤더(Cache-Control·ETag) + CDN
- XSS 공격 원리와 방어
- CSRF 공격 원리와 방어
- JWT 구조와 동작
- OAuth 2.0 흐름
- CSP + SameSite 정리
- 인증 흐름 전체 정리 (쿠키 기반 vs 토큰 기반)

### 코딩테스트
- 배열 기초 / 문자열 기초
- 해시맵·Set 활용
- 스택·큐 / 정렬
- DFS·BFS 기초
- 동적 프로그래밍 맛보기
- 그리디 기초
- 투 포인터·슬라이딩 윈도우 ①②

---

## Phase 2

### CS/브라우저 & 배포
- Critical Rendering Path / Reflow vs Repaint ✅
- 이벤트 루프 / Microtask vs Macrotask
- async·await 동작 원리
- 브라우저 저장소 비교 (Cookie·localStorage·sessionStorage·IndexedDB)
- 웹 성능 지표 (LCP·CLS·INP) + 캐시 지역성
- Linux 기본 명령어 / 파일 권한·SSH 키
- 빌드 도구 (Babel·Webpack·Vite)
- Docker 기초 / CI·CD·GitHub Actions
- 캐시 전략 심화 (stale-while-revalidate)

### 코딩테스트
- 구현·시뮬레이션 / 재귀 / 그래프 탐색
- 이분탐색 심화 / DP 심화 (LCS·LIS)
- 힙·우선순위 큐
- 스택·큐 심화 / 카카오 기출

---

## Phase 3

### CS/자료구조 & OS
- Big-O·시간·공간 복잡도
- 배열 vs 연결리스트 / 스택·큐·해시맵 원리
- 트리·BST·전위·중위·후위 순회
- 그래프·정렬 알고리즘
- 프로세스·스레드·Race Condition·뮤텍스
- 메모리 구조 (스택·힙) / 가비지 컬렉션·메모리 누수
- 가상 메모리·파일 시스템 / 포트·소켓·Web Worker

### 코딩테스트·테스트
- Jest 기초 / React Testing Library
- 비동기 테스트·Mock·MSW
- E2E 테스트 (Playwright)
- 투 포인터·슬라이딩 윈도우 심화 / 카카오 Lv.3 도전

---

## 폴더 구조

```
TIL/
└── CS/
    ├── Network/       # HTTP·DNS·TCP·CORS 등
    ├── Security/      # XSS·CSRF·JWT·OAuth 등
    ├── Browser/       # 렌더링·이벤트루프·성능 등
    ├── OS/            # 프로세스·메모리·파일시스템 등
    ├── DataStructure/ # 배열·트리·그래프·해시 등
    └── Algorithm/     # 정렬·탐색·DP·그리디 등
```

## 작성 규칙

- 파일명: 영어 소문자 하이픈 구분 — `http-methods.md`
- 커밋: `docs: Network - CORS 원리 정리`
- 복붙 금지, 이해한 내용을 내 말로
