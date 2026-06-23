# 큐로 주문 처리 (워밍업)

- **유형:** 큐 (FIFO)
- **목표:** 주문 배열을 받은 순서대로 처리·출력

## 문제

```javascript
function processOrders(orders) {
  // 직접 작성
}

processOrders(['커피', '케이크', '쿠키']);
// 처리 중: 커피
// 처리 중: 케이크
// 처리 중: 쿠키
```

## 접근 (왜 큐?)

- 먼저 들어온 주문을 먼저 처리(FIFO) → 앞에서 꺼내는 `shift`
- 흐름:
  1. orders를 queue로 복사
  2. queue가 빌 때까지 `shift`로 앞에서 하나씩 꺼내 출력

## 풀이

```javascript
function processOrders(orders) {
  const queue = [...orders];

  // ② queue가 비어있지 않은 동안 반복
  //    shift로 꺼내서 출력
  while (queue.length > 0){
    console.log(`처리 중: ${queue.shift()}`);
  }
}
```

## 자기 점검

- [ ] 출력이 순서대로 나오나? (커피 → 케이크 → 쿠키)
- [ ] `processOrders(['A','B','C','D'])` → A, B, C, D 순서

## 배운 점 / 막힌 점

- `pop`(뒤) vs `shift`(앞) 차이:
