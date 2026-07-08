# GitHub Actions

`.github/workflows/*.yml` 파일에 **"언제(on) → 무엇을(steps)"**을 적어두면 자동 실행되는 CI/CD 도구

---

## 예시 workflow

```yaml
name: Deploy Frontend

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - run: npm ci

      - run: npm run build

      - run: echo "빌드 완료, 서버로 배포"
```

---

## 한 줄씩 읽기

| 항목 | 뜻 |
|---|---|
| `name: Deploy Frontend` | 워크플로 이름. 그냥 라벨이라 아무거나 OK |
| `on: push: branches: [main]` | **언제 실행?** → main에 push되면 자동 실행 |
| `jobs: deploy:` | `deploy`라는 작업 하나 정의 (이름 자유) |
| `runs-on: ubuntu-latest` | 이 작업을 우분투(리눅스) 머신에서 돌려라 |
| `steps:` | 위 → 아래 순서로 실행 |

**steps 안:**
- `uses: actions/checkout@v4` → 코드 가져오기 (저장소 내용을 러너로 복사)
- `uses: actions/setup-node@v4` + `with: node-version: 20` → Node.js 20 설치
- `run: npm ci` → 패키지 설치
- `run: npm run build` → 빌드
- `run: echo "..."` → 메시지 출력 (실제론 여기에 배포 명령이 들어감)

---

## 한 문장 요약

> **main에 push되면 → 우분투에서 → 코드 받고 → Node 깔고 → 설치하고 → 빌드하는** 자동화.

---

## 읽는 요령

- **`uses`** = 남이 만든 기능 가져다 씀 (checkout, setup-node 같은 것)
- **`run`** = 터미널 명령어 직접 실행 (`npm ci` 등)
- **`with`** = 그 기능에 옵션 넣기 (node 버전 등)
