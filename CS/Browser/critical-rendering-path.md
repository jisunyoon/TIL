# Critical Rendering Path / Reflow vs Repaint

브라우저가 HTML을 받아서 화면에 픽셀을 그리기까지의 순서

---

## 1. Critical Rendering Path

```
HTML → DOM → CSSOM → Render Tree → Layout → Paint → Composite
```

**DOM** — 브라우저가 HTML 텍스트를 파싱해서 트리 구조 객체로 변환

**CSSOM** — CSS도 같은 방식으로 파싱해 객체 트리로 변환

**Render Tree** — DOM + CSSOM을 합쳐서 실제로 화면에 그려질 요소만 골라냄  
ex) `display: none` 요소는 Render Tree에 포함되지 않음

**Layout** — 각 요소가 화면 어디에, 얼마나 큰 크기로 위치할지 계산  
(Reflow라고도 부름)

**Paint** — 계산된 위치·크기에 따라 실제 픽셀을 화면에 그림

**Composite** — 여러 레이어를 합성해 최종 화면을 완성

---

## 2. Reflow vs Repaint

둘 다 렌더링 파이프라인이 **어느 단계부터 재실행되는지**를 의미한다.

| | 트리거 | 재실행 시작 단계 | 비용 |
|---|---|---|---|
| **Reflow** | 요소 크기·위치 변경 | Layout | 높음 |
| **Repaint** | 색상·배경 등 시각적 변경만 | Paint | 낮음 |

**Reflow** — `width`, `height`, `margin`, `padding`, `font-size` 등 레이아웃에 영향을 주는 속성이 바뀌면 발생  
Layout부터 다시 계산하기 때문에 성능 비용이 가장 큼

**Repaint** — `color`, `background-color`, `border-color` 등 시각적 속성만 바뀔 때 발생  
Layout은 건너뛰고 Paint부터 다시 실행하므로 Reflow보다 가벼움

> Reflow가 발생하면 Repaint도 항상 따라온다.  
> Repaint는 Reflow 없이 단독으로 발생할 수 있다.

---

## 3. 어느 단계를 CPU/GPU가 처리하나

| 단계 | 처리 주체 | 하는 일 |
|---|---|---|
| Layout (Reflow) | CPU | 위치·크기 계산 (제일 무거움) |
| Paint | CPU | 픽셀을 색칠해 레이어에 그림 |
| Composite | **GPU** | 이미 그려둔 레이어들을 합쳐 화면 출력 |

- `width`, `top` 등을 바꾸면 → CPU가 Layout부터 다시 계산 → 느림
- `transform`, `opacity`만 바꾸면 → Layout·Paint 건너뛰고 **GPU가 레이어만 다시 합성** → 빠름

> `transform`이 빠른 이유 = 이미 그려둔 그림(레이어)을 **GPU가 위치만 옮기는** 것이기 때문.  
> GPU는 이런 병렬 합성에 특화돼 있어 CPU보다 훨씬 매끄럽게 처리한다.

---

## 4. 성능 최적화 포인트

- `transform`, `opacity`는 Composite 단계만 재실행 → 가장 가벼움
- DOM 조작은 묶어서 한 번에 (Reflow 횟수 최소화)
- `display: none` 요소는 Render Tree에서 빠지므로 조작해도 Reflow 없음

---

## 5. Layout Thrashing (강제 동기 레이아웃)

레이아웃 값을 **읽기 → 쓰기 → 읽기 → 쓰기**로 번갈아 하면, 브라우저가 매번 Layout을 다시 계산해서 느려지는 현상.

**원리** — 브라우저는 스타일 변경을 모아뒀다 한 번에 Layout 하려는데(배칭),
`offsetHeight` · `getBoundingClientRect()` 같은 **레이아웃 값을 읽는 순간** "정확한 값 줘야 하니 지금 계산해!" 하고 쌓인 변경을 즉시 처리한다. (= 강제 동기 레이아웃)
이걸 쓰기와 번갈아 반복하면 매 루프마다 Layout이 강제 실행됨.

```js
// ❌ 읽기/쓰기 섞임 → 반복 계산
boxes.forEach(el => el.style.height = el.offsetHeight + 10 + 'px');

// ✅ 읽기 몰아서 → 쓰기 몰아서
const h = boxes.map(el => el.offsetHeight);
boxes.forEach((el, i) => el.style.height = h[i] + 10 + 'px');
```

> 규칙: **읽기는 읽기끼리, 쓰기는 쓰기끼리 몰아서** (read-then-write) → Layout이 한 번만 돈다.
