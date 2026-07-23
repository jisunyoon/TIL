# 체육복 (Lv.1, 그리디)

- **유형:** 그리디 (탐욕법 — 눈앞의 최선을 선택)
- **출처:** 프로그래머스

## 문제

```javascript
// 앞뒤 번호에게만 여벌을 빌려줄 수 있을 때, 수업 들을 수 있는 최대 학생 수
function solution(n, lost, reserve) {
  var answer = 0;
  return answer;
}

solution(5, [2, 4], [1, 3, 5]);  // 5
solution(5, [2, 4], [3]);        // 4
solution(3, [3], [1]);           // 2
```

## ⚠️ 함정: 여벌 가져온 학생이 도난당한 경우

- 예: `lost = [3]`, `reserve = [3]` → 3번은 여벌 2벌 중 1벌 도난
- → **자기 것 입으면 끝.** 잃은 것도 아니고, 빌려줄 수도 없음
- → **lost와 reserve 양쪽에 다 있는 번호는 서로 상쇄**시켜서 먼저 제거해야 함
  - `realLost` = lost 중 reserve에 없는 사람 (진짜 잃은 사람)
  - `realReserve` = reserve 중 lost에 없는 사람 (진짜 빌려줄 수 있는 사람)

## 그리디 아이디어

- 잃은 학생을 **번호 순서대로** 처리하면서:
  - **앞번호(l-1)** 여벌이 있으면 → 거기서 빌림 (우선!)
  - 없으면 **뒷번호(l+1)** 에서 빌림
- **왜 앞번호 먼저?** 뒷번호 여벌은 "그 다음 잃은 학생"도 쓸 수 있지만,
  앞번호 여벌은 지금 아니면 쓸 사람이 없음 → 앞부터 쓰는 게 항상 이득
- **정렬 필수:** 번호 순으로 처리해야 위 논리가 성립

## 새 도구

```javascript
arr.filter(x => 조건)     // 조건에 맞는 것만 남긴 새 배열
arr.includes(v)           // v가 배열에 있나? (true/false)
arr.indexOf(v)            // v의 위치(index), 없으면 -1
arr.splice(i, 1)          // i번째 원소 1개를 제거 (원본 변경)
arr.sort((a, b) => a - b) // 숫자 오름차순 정렬 (문자열 아님 주의!)
```

## 풀이

```javascript
function solution(n, lost, reserve) {

  const realLost = lost.filter(l => !reserve.includes(l));
  const realReserve = reserve.filter(r => !lost.includes(r));

  realLost.sort((a,b) => a - b);
  realReserve.sort((a,b) => a - b);

  let count = n - realLost.length;

  for(const l of realLost){
    let index = realReserve.indexOf(l - 1);
    if(index === -1) {
      index = realReserve.indexOf(l + 1);
    }
    if(index !== -1){
      count++;
      realReserve.splice(index,1);
    }
  }
  return count;
}
```

## 자기 점검

- [ ] `5, [2,4], [1,3,5]` → `5`
- [ ] `5, [2,4], [3]` → `4`
- [ ] `3, [3], [1]` → `2`
- [ ] `3, [1,2], [2,3]` → `2` (2번 상쇄 → 1번은 이웃인 2번이 못 빌려주고, 3번은 이웃이 아님)

## 배운 점 / 막힌 점

- lost∩reserve 상쇄를 왜 먼저 해야 하나:
- 왜 앞번호부터 빌리나 (그리디 근거):
- 왜 정렬이 필요한가:
- `filter`/`includes`/`indexOf`/`splice` 사용법:
- `sort((a,b) => a-b)` 숫자 정렬 (기본 sort는 문자열!):
