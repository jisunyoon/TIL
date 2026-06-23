# 괄호 회전하기 (실전, 스택)

- **유형:** 스택 (괄호 짝 매칭)
- **출처:** 프로그래머스

## 문제

```javascript
// 문자열을 0,1,2...칸 회전했을 때 올바른 괄호가 되는 경우의 수
function solution(s) {
  // 직접 작성
}

solution('[](){}'); // 3  (x=0,2,4일 때 올바름)
```

## 접근

- 두 부분으로 분리:
  1. 문자열 회전시키기
  2. 회전된 문자열이 **올바른 괄호인지** 확인 (스택)
- 모든 x(0 ~ length-1)에 대해 반복하며 count

## 풀이

```javascript
function solution(s) {
  let count = 0;

  for (let x = 0; x < s.length; x++) {
    // ② x칸 회전된 문자열 만들기
    //    힌트: s.slice(x) + s.slice(0, x)


    // ③ 올바른 괄호면 count++
  }

  return count;
}

function isValid(s) {
  const stack = [];
  const pair = { ')': '(', ']': '[', '}': '{' };

  // ④ 각 글자 순회
  //    - 여는 괄호면 push
  //    - 닫는 괄호면 stack.pop()이 pair[ch]와 같은지 확인, 다르면 false


  // ⑤ 스택이 비어있으면 true
  return stack.length === 0;
}
```

## 자기 점검

- [ ] `'[](){}'` → `3`
- [ ] `'}]()[{'` → `2`
- [ ] `'[)(]'` → `0`

## 배운 점 / 막힌 점

- 회전 로직 `s.slice(x) + s.slice(0, x)`:
- 짝 매칭 패턴:
- 시간복잡도:
