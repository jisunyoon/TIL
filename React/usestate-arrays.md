# useState 심화 — 배열 상태와 불변성


## 학습 목표

- 배열 상태 추가 / 삭제 / 수정
- 왜 `push`가 아니라 spread인지

## 정리
우선 불변성이란 배열/객체 상태는 직접 건드리면 안 되고, 새로 만들어야 React가 감지한다. 
push는 원본을 그대로 두고 안만 바꿔서 못알아챈다.

배열 상태 3가지 조작 
1. 추가(spread로 뒤에 붙이기)
setTodos([...todos, newTodo]);
//         ↑기존 것들 펼치고  ↑새 거 추가

2. 삭제(filter로 걸러내기)
setTodos(todos.filter(todo) => todo.id !== 삭제할 id);
→ filter는 원본 안 건드리고 새 배열 반환하니까 불변성에 딱.

3. 수정(map으로 갈아끼우기)
setTodos(todos.map(todo =>
    todo.id === 수정할 id
    ? { ...todo, done: !todo.done }   // 이 항목만 새 객체로 교체
    : todo                            // 나머진 그대로
))


객체도 마찬가지
객체도 배열과 동일 — 원본 수정 ❌, 새로 만들기 ✅

user.age = 21; setUser(user);      // ❌ 같은 주소
setUser({ ...user, age: 21 });     // ✅ 새 주소

- { ...obj }로 기존 속성 먼저 펼치기 (안 하면 나머지 날아감)
- 바꿀 것만 뒤에 덮어쓰기: { ...obj, 키: 값 }

## 실습

`javascript-mastery/mini-projects/react-playground` — `TodoList.tsx`

## 확인한 것

<!-- 직접 돌려보고 알게 된 것 -->
