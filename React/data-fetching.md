# fetch와 async/await — 데이터 가져오기


## 학습 목표

- Promise의 3가지 상태와 `then` / `catch`
- `async` / `await`가 Promise를 어떻게 바꿔 쓰는지
- `fetch`의 함정 (`res.ok`를 왜 직접 확인해야 하는지)
- 로딩 / 에러 / 데이터 3가지 상태 다루기

## 정리
Promise는 나중에 나올 결과를 담아놓는 곳
아래와 같이 3가지 상태가 있다.
 "Pending(대기)", "Fulfilled(이행/성공)", "Rejected(거부/실패)"
→ 처음엔 무조건 Pending, 그다음 성공하면 Fulfilled, 실패하면 Rejected로 딱 한 번 바뀐다.

---------------------------------------------

async/await
async로 비동기 함수야라는 걸 표시하고, await으로 가져올 때까지 기다린다. fetch로 서버에서 가져오고, json으로 꺼낸다.

(흐름 이해)
async function getData() {           // async: 비동기 표시
  const res = await fetch(url);      // await + fetch: 요청하고 기다림
  const data = await res.json();     // await + json: 데이터 꺼냄
  return data;
}

---------------------------------------------

fetch의 함정 (axios는 함정이 없음)
fetch는 서버가 에러 응답(404,500)을 줘도 "성공"을 처리한다.
-> 서버가 "그런 페이지 없어요(404)"라고 해도, fetch는 "응답은 받았으니 성공!" 이라고 생각함

fetch가 실패로 치는 경우 vs 아닌 경우

fetch가 catch로 가는 경우 (진짜 실패):
- 네트워크 끊김 (인터넷 안 됨)
- 주소 자체가 잘못됨 (도메인 없음)
- CORS 차단
→ "응답을 아예 못 받은" 경우만 에러로 침.

fetch가 성공으로 치는 경우 (근데 사실 문제):

- 404 (페이지 없음)
- 500 (서버 에러)
- 403 (권한 없음)

→ "응답은 받았는데 내용이 에러" 인 경우는 성공으로 봄.

그래서 res.ok를 직접 확인한다. 
res.ok는 false면 직접 throw로 에러를 던져서 catch로 보낸다. 아니면 에러인데도 그냥 넘어가버림.
res.ok = true   → 상태 코드 200~299 (성공)
res.ok = false  → 400, 500 등 (에러)

(예시)
const res = await fetch(url);

if (!res.ok) {                    // ← 직접 체크!
  throw new Error('요청 실패');   // 에러 강제로 던지기 → catch로 감
}

const data = await res.json();

---------------------------------------------

fetch 처리 2가지 방법

방법 1: .then / .catch
function getPosts() {
    setLoading(true);
    fetch(url)
    .then(res => {
        if(!res.ok) throw new Error('실패');
        return res.json();
    })
    .then(data => setPosts(data))      // 성공
    .catch(err => setError(err.message)) // 실패
    .finally(() => setLoading(false));   // 무조건
}

방법 2: async / await + try / catch
async function getPosts(){
    setLoading(true);
    try{
        const res = await fetch(url);
        if (!res.ok) throw new Error('실패');
        const data = await res.json();
        setPosts(data);                  // 성공
    }catch(err){
        setError(err.message);           // 실패
    }finally{
        setLoading(false);               // 무조건
    }
}



## 실습

`javascript-mastery/mini-projects/react-playground` — `Blog.tsx`

## 확인한 것

<!-- 직접 돌려보고 알게 된 것 -->
