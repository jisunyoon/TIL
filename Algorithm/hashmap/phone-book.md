# 전화번호부 (실전, 해시맵/문자열)

- **유형:** 해시맵 / 문자열 (접두어 검사)
- **출처:** 프로그래머스

## 문제

```javascript
// 한 번호가 다른 번호의 접두어인 경우가 있으면 false, 없으면 true
function solution(phone_book) {
  // 직접 작성
}

solution(['119', '97674223', '1195524421']);       // false ('119'가 '1195...'의 접두어)
solution(['123', '456', '789']);                    // true
solution(['12', '123', '1235', '567', '88']);       // false ('12'가 '123'의 접두어)
```

## 접근

- **완전탐색(모든 쌍 비교)은 O(n²)** → n이 100만이라 시간 초과
- 아이디어 두 가지:

**① 정렬 활용 (추천, O(n log n))**
- 문자열 정렬하면 접두어 관계인 번호는 **바로 옆(인접)** 에 온다
  - 예: `['119', '1195524421', ...]` → `'119'` 다음에 `'1195...'`
- → **인접한 쌍만** 검사하면 충분 (`startsWith`)

**② Set + 접두사 검사 (해시, O(n·L))**
- 모든 번호를 Set에 넣고
- 각 번호의 **모든 접두사**(1글자, 2글자, ...)가 Set에 있는지 확인
- L은 번호 길이(최대 20)라 상수 취급

## 풀이 — ① 정렬

```javascript
function solution(phone_book) {
  const p = phone_book.sort();

  for(let i = 0; i < p.length -1; i++){
    if(p[i + 1].startsWith(p[i])){
      return false;
    }
  }


  return true;
}
```

## 다른 풀이 — ② Set

```javascript
function solution(phone_book) {
  const set = new Set(phone_book);

  for (const number of phone_book) {
    for(let i = 1; i < number.length; i++){
      const p = number.slice(0, i);
      if(set.has(p)) return false;
    }
  }

  return true;
}
```

## 자기 점검

- [ ] `['119','97674223','1195524421']` → `false`
- [ ] `['123','456','789']` → `true`
- [ ] `['12','123','1235','567','88']` → `false`

## 배운 점 / 막힌 점

- 왜 O(n²) 완전탐색이 안 되는가 (n = 100만):
- 정렬하면 접두어가 인접하는 이유:
- `startsWith` vs `slice` 접두사 비교:
- 시간복잡도:
