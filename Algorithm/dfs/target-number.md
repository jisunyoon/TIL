# 타겟 넘버 (Lv.2, DFS/재귀)

- **유형:** DFS (깊이 우선 탐색) / 재귀
- **출처:** 프로그래머스

## 문제

```javascript
// 각 숫자를 +/- 해서 target을 만드는 방법의 수
function solution(numbers, target) {
  var answer = 0;
  return answer;
}

solution([1, 1, 1, 1, 1], 3);   // 5
solution([4, 1, 2, 1], 4);      // 2
```

## 핵심 아이디어: 모든 +/- 조합을 다 해본다

- 각 숫자마다 **두 갈래**: `+` 하거나 `-` 하거나
- 숫자 n개면 → `2 × 2 × ... = 2^n` 가지 경우 (최대 2^20 ≈ 100만, 완전탐색 OK)
- 이 "갈림길마다 두 갈래로 뻗는" 구조를 **재귀(DFS)** 로 자연스럽게 표현

## 재귀(DFS)란?

- **자기 자신을 다시 부르는 함수.** "다음 숫자로 넘어가서 또 +/- 갈라짐"을 반복
- 두 가지가 반드시 필요:
  1. **종료 조건(끝):** 숫자를 다 썼을 때 → 합이 target이면 성공(1가지)
  2. **갈라지기(재귀 호출):** 지금 숫자를 더한 경우 + 뺀 경우, 둘 다 탐색

## 그림 (트리처럼 갈라짐)

```
[1, 1]  target=0 예시

          시작(합0, index0)
         /               \
      +1(합1)           -1(합-1)      ← 첫 숫자 +/-
      /    \            /     \
   +1(2)  -1(0)✅    +1(0)✅  -1(-2)   ← 둘째 숫자 +/-
                                        (index=2, 다 씀 → 합==0 인 것만 카운트)
```

## 풀이 (재귀)

```javascript
function solution(numbers, target) {
  let count = 0;

  // dfs(index, sum): index번째 숫자를 볼 차례, 지금까지 합이 sum
  function dfs(index, sum) {
    // 종료 
    if(index === numbers.length){
      if(sum === target) count++;
      return;
    }
    // sum = 누적 numbers[index] 이번 숫자
    dfs(index + 1, sum + numbers[index]);
    dfs(index + 1, sum - numbers[index]);
  }

  dfs(0, 0); 
  return count;
}
```

## 자기 점검

- [ ] `[1,1,1,1,1]`, `3` → `5`
- [ ] `[4,1,2,1]`, `4` → `2`
- [ ] `[1,1]`, `2` → `1` (`+1+1`만)

## 배운 점 / 막힌 점

- 재귀의 두 요소 (종료 조건 + 재귀 호출):
- 왜 `index === numbers.length`가 끝인가:
- 각 숫자마다 +/- 두 번 호출 → 트리처럼 2^n 갈래:
- `count`를 바깥에 두고 재귀 안에서 올리는 이유:
