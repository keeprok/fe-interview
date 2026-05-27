# ⚡ JavaScript 비동기

## 목차
- [이벤트 루프](#이벤트-루프)
- [Microtask Queue 동작 과정](#microtask-queue-동작-과정)
- [Promise와 Async/Await의 차이점](#promise와-asyncawait의-차이점)
- [비동기 로직 동작 원리](#비동기-로직-동작-원리)

---

## 이벤트 루프

> JavaScript 이벤트 루프(Event Loop)에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

JavaScript는 **싱글 스레드** 언어지만, 이벤트 루프를 통해 비동기 처리가 가능합니다.

**구성 요소:**
- **Call Stack**: 현재 실행 중인 함수들의 스택
- **Web APIs**: 브라우저가 제공하는 API (setTimeout, fetch, DOM 이벤트 등)
- **Callback Queue (Task Queue)**: setTimeout, setInterval 등의 콜백 대기
- **Microtask Queue**: Promise, MutationObserver 등의 콜백 대기
- **Event Loop**: Call Stack이 비면 Queue에서 꺼내 실행

**우선순위:** Microtask Queue > Callback Queue

```js
console.log('1');

setTimeout(() => console.log('2'), 0); // Callback Queue

Promise.resolve().then(() => console.log('3')); // Microtask Queue

console.log('4');

// 출력 순서: 1 → 4 → 3 → 2
```

</details>

---

## Microtask Queue 동작 과정

> Microtask Queue의 동작 과정을 설명해주세요.

<details>
<summary>답변 보기</summary>

**Microtask Queue**는 일반 Task Queue보다 **높은 우선순위**를 가집니다.

**Microtask 생성 시점:**
- `Promise.then/catch/finally`
- `async/await` (내부적으로 Promise 사용)
- `queueMicrotask()`
- `MutationObserver`

**처리 순서:**
1. Call Stack의 현재 태스크 실행 완료
2. **Microtask Queue 완전히 비울 때까지** 실행 (새로운 microtask가 추가되어도 계속 처리)
3. 브라우저 렌더링 (필요한 경우)
4. Callback Queue에서 하나 꺼내 실행
5. 다시 2번부터 반복

```js
setTimeout(() => console.log('setTimeout'), 0);

Promise.resolve()
  .then(() => {
    console.log('Promise 1');
    return Promise.resolve();
  })
  .then(() => console.log('Promise 2'));

// 출력: Promise 1 → Promise 2 → setTimeout
// Microtask Queue가 모두 비워진 후 setTimeout 실행
```

</details>

---

## Promise와 Async/Await의 차이점

> Promise와 Async/Await의 차이점과 각각의 장단점을 설명해주세요.

<details>
<summary>답변 보기</summary>

**Promise:**
```js
fetchUser()
  .then(user => fetchPosts(user.id))
  .then(posts => console.log(posts))
  .catch(err => console.error(err));
```

**Async/Await:**
```js
async function loadData() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    console.log(posts);
  } catch (err) {
    console.error(err);
  }
}
```

| 구분 | Promise | Async/Await |
|------|---------|-------------|
| 가독성 | 체이닝이 길어지면 복잡 | 동기 코드처럼 읽혀 직관적 |
| 에러 처리 | `.catch()` | `try/catch` |
| 병렬 처리 | `Promise.all()` 자연스러움 | `await Promise.all()` |
| 디버깅 | 스택 트레이스 파악 어려움 | 동기처럼 디버깅 가능 |

**async/await는 Promise의 문법적 설탕(syntactic sugar)**입니다. 내부적으로 Promise를 사용합니다.

**병렬 처리:**
```js
// 순차 실행 (느림)
const a = await fetchA();
const b = await fetchB();

// 병렬 실행 (빠름)
const [a, b] = await Promise.all([fetchA(), fetchB()]);
```

</details>

---

## 비동기 로직 동작 원리

> JavaScript의 비동기 처리가 어떻게 동작하는지 설명해주세요.

<details>
<summary>답변 보기</summary>

JavaScript는 싱글 스레드이지만, **브라우저(또는 Node.js)가 제공하는 Web API**를 통해 비동기 작업을 처리합니다.

**동작 흐름:**
1. JS 엔진이 `setTimeout(callback, 1000)` 실행
2. **Web API**가 1000ms 타이머를 백그라운드에서 처리 (JS 스레드 아님)
3. 1000ms 후 callback을 **Callback Queue**에 추가
4. **Event Loop**가 Call Stack이 비어있는지 확인
5. 비어있으면 Queue에서 callback을 꺼내 Call Stack에 추가
6. callback 실행

**핵심:** JS 코드 자체는 싱글 스레드로 실행되지만, I/O 작업(네트워크, 타이머 등)은 브라우저/OS가 멀티스레드로 처리합니다.

</details>
