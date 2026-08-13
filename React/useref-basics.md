# useRef 동작 원리


## 학습 목표

- useRef가 useState와 다른 점 (리렌더링을 일으키지 않는다)
- 언제 useRef를 쓰고 언제 useState를 쓰는지
- DOM 직접 접근
- 타이머 id 보관 + `clearInterval` 클린업

## 정리
useRef가 useState와 다른 점
useState는 값이 바뀌면 리렌더링을 일으키고, useRef는 값이 바뀌어도 리렌더링을 일으키지 않는다.
즉,
값이 바뀔때 화면도 바뀌어야 하면 useState, 화면과 무관하면 useRef

(ex)
useState → 화면에 보여줄 값 (카운트, 입력값, 목록)
useRef   → 화면 뒤에서 쓸 값 (타이머 ID, DOM 참조, 이전 값 저장)

공통점: 둘 다 리렌더돼도 값을 기억한다.

## 실습

`javascript-mastery/mini-projects/react-playground` — `Timer.tsx`

## 확인한 것

<!-- 직접 돌려보고 알게 된 것 -->
