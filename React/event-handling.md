# 이벤트 처리


## 학습 목표

- onClick / onChange / onSubmit
- 합성 이벤트(SyntheticEvent)와 DOM 이벤트의 차이

## 정리

이벤트 처리란 사용자의 행동(클릭, 입력, 제출)에 반응하여 코드를 실행하는 것을 뜻한다. 

onClick - 클릭했을 때
onClick에 함수를 넘겨준다. 클릭하는 순간 그 함수가 실행 됨 
<button onClick={handleClick}>눌러</button>
[주의]
<button onClick={handleClick}>     // ✅ 함수만 넘김
<button onClick={handleClick()}>   // ❌ ()붙이면 렌더링 때 바로 실행됨
<button onClick={() => handleClick(값)}>  // ✅ 인자 넘길 땐 화살표로 감싸기


onChange - 입력값이 바뀔 때 
e.target.value = 사용자가 입력창에 친 현재 값
글자 하나 칠 때마다 onChange가 실행돼서 상태 업데이트 

const [text, setText] = useState('');
<input value={text} onChange={handleChange} />
function handleChange(e) {
  setText(e.target.value);   // 입력한 값을 상태에 저장
}

이걸 "제어 컴포넌트"라고 한다. 
입력값을 react 상태로 통제하는 구조 

onSubmit - 폼 제출할 때 
e.preventDefault() 꼭 넣기!
폼은 원래 제출하면 페이지가 새로고침됨 (HTML 기본 동작)
→ preventDefault()로 막아야 React가 처리 가능

<form onSubmit={handleSubmit}>
  <input value={text} onChange={handleChange} />
  <button type="submit">보내기</button>
</form>

function handleSubmit(e) {
  e.preventDefault();   // 필수! 새로고침 막기
  console.log('제출:', text);
}

event 객체
이벤트 핸들러는 e(이벤트 정보)를 받는다. 

e.target.value // 입력값
e.target // 이벤트가 일어난 요소 
e.preventDefault() // 기본 동작 막기 
e.clientX, e.clientY // 마우스 좌표

합성 이벤트(SyntheticEvent)란?
React가 브라우저의 진짜 DOM 이벤트를 감싸서 만든 "React용 이벤트 객체"
1. 브라우저마다 다른 걸 통일해줘
2. 성능 최적화 (이벤트 위임 =  최상위에 리스너 하나만 → 어디서 일어났는지 판단 (가벼움))

DOM 이벤트 vs 합성 이벤트 비교

	            DOM 이벤트 (순수 JS)	    합성 이벤트 (React)
등록	          addEventListener	         onClick={...}
이름	          소문자 click	              카멜케이스 onClick
브라우저 차이	   있음	                       React가 통일 
성능	          리스너 각각	               이벤트 위임 

이름 규칙 차이 
DOM:   click, change, submit, mousemove  (소문자)
React: onClick, onChange, onSubmit, onMouseMove  (camelCase)


마우스 좌표 세 가지
같은 자리를 가리켜도 "어디를 기준으로 재느냐"가 달라서 값이 다르다.

client : 지금 보이는 화면(뷰포트) 좌상단 기준  → 스크롤 영향 X
page   : 문서(document) 전체 좌상단 기준       → 스크롤 영향 O
screen : 모니터 물리 화면 좌상단 기준          → 스크롤 영향 X

스크롤을 100px 내리고 같은 자리를 가리키면
- client.y : 그대로 (보이는 화면 기준이라)
- page.y   : 100 늘어남 (문서 기준이라 위로 밀려난 만큼 더해짐)
- screen.y : 그대로 (브라우저 창 위치는 안 변했으니)

언제 뭘 쓰나
client : 화면에 띄우는 것 (툴팁, 커서 따라다니는 요소) — position: fixed 와 짝
page   : 문서 위 절대 위치가 필요할 때 — position: absolute 와 짝
screen : 거의 안 씀 (멀티 모니터 등)


e.nativeEvent
e는 React가 감싼 껍데기(SyntheticEvent)라, 원본 DOM 이벤트의 속성이 전부 있진 않다.
React가 안 챙겨준 게 필요하면 e.nativeEvent 를 거쳐서 원본에서 꺼낸다.

e.offsetX              // ❌ React 껍데기엔 없음
e.nativeEvent.offsetX  // ✅ 원본 객체로 들어가서 꺼냄

offsetX/offsetY = 이벤트가 일어난 "그 요소"의 좌상단 기준 좌표
→ "이 div 안에서 몇 px 지점인지"가 필요할 때

챙겨주는 것 : clientX/Y, pageX/Y, screenX/Y, movementX/Y, button, altKey, shiftKey ...
안 챙기는 것 : offsetX/Y, layerX/Y ... → nativeEvent 로


e.target vs e.currentTarget
이벤트 위임 때문에 이 둘이 다를 수 있다.

target        : 실제로 이벤트가 발생한 요소 (제일 안쪽, 내가 진짜 누른 것)
currentTarget : 핸들러가 붙어 있는 요소

<div onClick={handleClick}>      // currentTarget = div
  <button>눌러</button>           // 버튼을 누르면 target = button
</div>

핸들러를 붙인 요소 자체가 필요하면 currentTarget 을 써야 안전하다.


## 실습

`javascript-mastery/mini-projects/react-playground` — `MouseTracker.tsx`

## 확인한 것

<!-- 직접 돌려보고 알게 된 것 -->
