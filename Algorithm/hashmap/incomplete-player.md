# 완주하지 못한 선수 (실전, 해시맵)

- **유형:** 해시맵 (개수 카운트)
- **출처:** 프로그래머스

## 문제

```javascript
// 참가자 중 완주하지 못한 한 명 찾기 (동명이인 있음)
function solution(participant, completion) {
  // 직접 작성
}

solution(['leo', 'kiki', 'eden'], ['eden', 'kiki']);            // 'leo'
solution(['mislav','stanko','mislav','ana'], ['stanko','ana','mislav']); // 'mislav'
```

## 접근

- 단순 비교(`includes`)는 **동명이인 처리 불가** → 개수를 세서 비교
- 흐름:
  1. participant 카운트 → `{ mislav:2, stanko:1, ana:1 }`
  2. completion으로 -1 → `{ mislav:1, stanko:0, ana:0 }`
  3. 개수가 0이 아닌 사람 return

## 풀이

```javascript
function solution(participant, completion) {
  const map = {};

  // ① participant 순회하며 카운트
  //    힌트: map[name] = (map[name] || 0) + 1;
  for(let name of participant){
    map[name] = (map[name] || 0) + 1;
  }


  // ② completion 순회하며 -1
  for(let name of completion){
    map[name]--;
  }



  // ③ 개수가 0이 아닌 사람 찾아서 return
  for(let name in map){
    if(map[name] > 0){
      return name
    }
  }
}
```

## 다른 풀이 (sort)

- 아이디어: 둘 다 정렬하면 완주자끼리 같은 위치 → 처음 어긋나는 곳이 답
- 시간복잡도: O(n log n) (해시맵 O(n)보다 느리지만 직관적)

```javascript
function solution(participant, completion) {
  participant.sort();
  completion.sort();

  // ① 같은 인덱스끼리 비교, 다르면 participant[i] return
  for(let i = 0; i < participant.length; i++){
    if(participant[i] !== completion[i]){
      return participant[i];
    }
  }


  // ② 끝까지 같으면 → participant의 마지막 사람 return
  return participant[participant.length -1];
}
```

## 자기 점검

- [ ] `['leo','kiki','eden'] / ['eden','kiki']` → `'leo'`
- [ ] 동명이인 → `'mislav'`
- [ ] `solution(['leo'], [])` → `'leo'`

## 배운 점 / 막힌 점

- 첫 시도(`includes`)가 안 된 이유:
- `map[name] = (map[name] || 0) + 1` 패턴:
- 시간복잡도:
