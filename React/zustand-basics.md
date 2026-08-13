# Zustand — 전역 상태 관리


## 학습 목표

- `create` 안에 상태와 액션을 같이 쓰는 구조
- `set` / `get`의 역할
- 셀렉터로 필요한 것만 구독하기 (불필요한 리렌더 막기)
- 배열 상태 불변성 (담기 / 삭제 / 수량 변경)

## 정리
zustand란 여러 컴포넌트가 함께 쓰는 상태를 한 곳에서 관리하는 도구 (상태 관리 라이브러리)
Zustand는 전역 저장소에 두고 어디서든 꺼내 쓰게 해줘. (예를 들어, 장바구니 데이터 등 여러 컴포넌트가 다 필요로 하는 걸 zustand로 상태 관리)

보일러 플레이트(반복 코드)가 적어진다.

---------------------------------------------

기본 문법
create = 저장소 만들기 
set = 상태를 바꾸는 함수(useState의 setState 같은 것)
상태 + 액션을 한 객체 안에 같이 쓴다. 

create((set) => ({
    상태: 값,
    액션: () => set(...);
}))

(ex)
import { create } from 'zustand';

const useStore = create((set) => ({
  // 상태
  count: 0,

  // 액션 (상태 바꾸는 함수)
  increase: () => set((state) => ({ count: state.count + 1 })),
  reset: () => set({ count: 0 }),
}));

여기서 state는 "현재 상태 전체"를 말한다. 
즉 set안에서 (state) => 로 받는 state는 지금 저장소에 들어있는 상태 전부야 

1. 언제 state가 필요하냐면 - 이전 값을 기반으로 바꿀 때 

// 현재 count에 +1 → 이전 값(state.count)을 알아야 함
increase: () => set((state) => ({ count: state.count + 1 }))

// 배열에 추가 → 기존 items를 알아야 함
addItem: (item) => set((state) => ({ items: [...state.items, item] }))

2. 필요 없는 경우 
// 그냥 0으로 리셋 → 이전 값 필요 없음
reset: () => set({ count: 0 })

// 그냥 비우기 → 이전 값 필요 없음
clear: () => set({ items: [] })

---------------------------------------------

get은 현재 상태를 "읽어오는" 함수 set이 "쓰기"이면 get은 "읽기"이다.

create((set, get) => ({...}))
- set = 상태 쓰기(변경)
- get = 상태 읽기

get 쓰는 경우:
- set 밖에서 현재 값이 필요할 때
- 여러 상태 조합해 계산할 때 (총액, 총개수)

getTotalPrice: () => {
  const items = get().items;
  return items.reduce((sum, i) => sum + i.price * i.quantity, 0);
}

---------------------------------------------

셀렉터 - 저장소에서 필요한 것만 콕 집어서 가져오는 것.

const items = useCartStore((state) => state.items);  // items만
→ 그 값 바뀔 때만 리렌더 (전체 구독하면 불필요한 리렌더)
예: user만 바뀌어도, items만 쓰는 컴포넌트는 리렌더 안 함

컴포넌트에서 사용

function Cart() {
  const items = useCartStore((state) => state.items);
  const addItem = useCartStore((state) => state.addItem);
  const removeItem = useCartStore((state) => state.removeItem);

  return (...)
}

→ 셀렉터로 상태·액션을 각각 꺼내 씀 (props 안 내려도 어디서든 가능)


## 실습

`javascript-mastery/mini-projects/react-playground` — `cartStore.ts`, `Cart.tsx`

## 확인한 것

<!-- 직접 돌려보고 알게 된 것 -->
