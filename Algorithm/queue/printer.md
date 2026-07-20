# 프린터 (Lv.2, 큐)

- **유형:** 큐 (FIFO) + 우선순위 비교
- **출처:** 프로그래머스

## 문제

```javascript
// 중요도 높은 문서 먼저 인쇄. 내가 요청한 문서(location)가 몇 번째로 인쇄되나?
function solution(priorities, location) {
  var answer = 0;
  return answer;
}

solution([2, 1, 3, 2], 2);          // 1  (index 2의 '3'이 첫 번째로 인쇄)
solution([1, 1, 9, 1, 1, 1], 0);    // 5
```

## 인쇄 규칙

1. 맨 앞 문서 J를 꺼냄
2. 남은 것 중 J보다 **중요도 높은 게 하나라도 있으면** → J를 **맨 뒤로** 보냄
3. 없으면 → J를 **인쇄** (인쇄 순번 +1)

## 핵심 고민: "내 문서"를 어떻게 추적하나?

- 문서들이 큐 안에서 **앞↔뒤로 계속 움직임** → 위치(index)가 바뀜
- 그냥 중요도만 담으면 `[2,1,3,2]`에서 **어느 2가 내 문서(location=2)인지** 구분 불가
- **해결:** 각 문서를 `{ 중요도, 원래위치 }` 로 묶어서 큐에 넣음
  - 인쇄할 때 그 문서의 `원래위치 === location` 이면 → 내 문서!

## 접근

- 큐 원소 = `{ priority, index }` (index = 원래 위치)
- 남은 것 중 **더 높은 중요도가 있나?** → `Math.max(...남은중요도들)` 와 비교
- 흐름:
  1. 맨 앞을 꺼냄(`shift`)
  2. 큐에 남은 것 중 더 높은 중요도가 있으면 → 다시 맨 뒤로(`push`)
  3. 없으면 → 인쇄(count++), 그게 내 문서(`index === location`)면 count return

## 풀이

```javascript
function solution(priorities, location) {
  const queue = priorities.map((p,index) => [p,index]);
  let count = 0;

  while(queue.length){
     const now = queue.shift();

     if(queue.some(x => x[0] > now[0])){ // x에 나보다 큰게 있으면 걍 뒤로 넘김
      queue.push(now);
     }else{ // 나보다 큰게 없으면
        count++;
        if(now[1] === location) return count;
     }

  }
}
```

## 자기 점검

- [ ] `[2,1,3,2]`, `2` → `1`
- [ ] `[1,1,9,1,1,1]`, `0` → `5`
- [ ] `[1]`, `0` → `1` (문서 1개)

## 배운 점 / 막힌 점

- 왜 중요도만 담으면 안 되나 (내 문서 추적):
- `map`으로 index 붙이기 / `some`으로 "하나라도 있나" 검사:
- `shift`(앞 꺼내기) + `push`(뒤로 보내기) = 큐 회전:
- 시간복잡도:
