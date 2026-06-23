# 스택으로 문자열 뒤집기 (워밍업)

- **유형:** 스택 (LIFO)
- **목표:** `'hello'` → `'olleh'`

## 문제

```javascript
// 'hello' → 'olleh'로 만드는 함수
function reverse(str) {
  // 직접 작성
}

console.log(reverse('hello'));  // 'olleh'
console.log(reverse('world'));  // 'dlrow'
```

## 접근 (왜 스택?)

- 글자를 순서대로 push → 꺼낼 땐 마지막 글자부터 나옴(LIFO) → 자연스럽게 역순
- 흐름:
  1. 각 글자를 stack에 push → `['h','e','l','l','o']`
  2. pop하며 result에 누적 → `'o' → 'ol' → ... → 'olleh'`
  3. result return

## 풀이

```javascript
function reverse(str) {
  const stack = [];

  for(let s of str){
    stack.push(s);
  }

  let result = '';

  while(stack.length > 0) {
    result +=  stack.pop();
  }

  return result;
}
```

## 자기 점검

- [ ] `reverse('a')` → `'a'`
- [ ] `reverse('지선')` → `'선지'`
- [ ] `reverse('javascript')` → `'tpircsavaj'`

## 배운 점 / 막힌 점

-
