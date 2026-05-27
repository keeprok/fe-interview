# ⚛️ React 핵심 개념

## 목차
- [React의 장단점](#react의-장단점)
- [라이프사이클](#라이프사이클)
- [원시값과 참조값의 차이 (메모리 관점)](#원시값과-참조값의-차이-메모리-관점)
- [setState는 동기? 비동기?](#setstate는-동기-비동기)
- [React 18과 이전의 차이](#react-18과-이전의-차이)
- [좋은 컴포넌트란](#좋은-컴포넌트란)
- [Key Props 사용 이유](#key-props-사용-이유)
- [Flux와 MVC, CSR, SSR 구조](#flux와-mvc-csr-ssr-구조)

---

## React의 장단점

> React의 장점과 단점을 설명해주세요.

<details>
<summary>답변 보기</summary>

**장점:**
- **Virtual DOM**으로 효율적인 렌더링
- **컴포넌트 기반** 개발로 재사용성과 유지보수성 향상
- **단방향 데이터 흐름**으로 예측 가능한 상태 관리
- 방대한 생태계와 커뮤니티
- React Native로 모바일 개발 가능

**단점:**
- UI 라이브러리이므로 라우팅, 상태관리 등 추가 라이브러리 필요
- JSX 문법 학습 곡선
- 잦은 업데이트로 마이그레이션 비용 발생
- 번들 크기가 상대적으로 큼

</details>

---

## 라이프사이클

> React 컴포넌트의 라이프사이클에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

**클래스 컴포넌트 라이프사이클:**
1. **Mount**: `constructor` → `render` → DOM 업데이트 → `componentDidMount`
2. **Update**: `render` → DOM 업데이트 → `componentDidUpdate`
3. **Unmount**: `componentWillUnmount`

**함수 컴포넌트 (Hooks):**
```js
useEffect(() => {
  // componentDidMount + componentDidUpdate
  console.log('마운트 또는 업데이트');

  return () => {
    // componentWillUnmount (클린업)
    console.log('언마운트');
  };
}, [dependency]);

useEffect(() => {
  // componentDidMount only
}, []);
```

| 클래스 | Hooks |
|--------|-------|
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [dep])` |
| `componentWillUnmount` | `useEffect(() => { return cleanup }, [])` |

</details>

---

## 원시값과 참조값의 차이 (메모리 관점)

> 원시값과 참조값(array, object)의 차이점을 메모리 관점에서 설명해주세요.

<details>
<summary>답변 보기</summary>

**원시값(Primitive):** `string`, `number`, `boolean`, `null`, `undefined`, `Symbol`, `BigInt`
- 스택(Stack) 메모리에 **값 자체** 저장
- 불변(immutable)
- 할당/비교 시 **값 복사**

**참조값(Reference):** `object`, `array`, `function`
- 힙(Heap) 메모리에 실제 데이터 저장
- 스택에는 힙의 **메모리 주소(참조값)** 저장
- 할당/비교 시 **주소 복사**

```js
// 원시값: 값 비교
const a = 'hello';
const b = 'hello';
console.log(a === b); // true (값이 같음)

// 참조값: 주소 비교
const obj1 = { x: 1 };
const obj2 = { x: 1 };
console.log(obj1 === obj2); // false (다른 주소)
const obj3 = obj1;
console.log(obj1 === obj3); // true (같은 주소)
```

**React에서의 영향:**
- `useEffect`, `useMemo`, `useCallback`의 dependency array에서 객체/배열은 매 렌더마다 새 참조가 생겨 무한 루프 발생 가능
- `useState`에서 객체 업데이트 시 반드시 새 참조 생성 필요 (`{...prev, key: value}`)

</details>

---

## setState는 동기? 비동기?

> setState는 동기로 동작하나요, 비동기로 동작하나요? setState가 적용되는 과정을 설명해주세요.

<details>
<summary>답변 보기</summary>

`setState`는 **비동기적으로 동작**합니다.

**이유:** React는 성능 최적화를 위해 여러 setState 호출을 **배치(batching)** 처리합니다.

```js
function handleClick() {
  setCount(count + 1);
  setCount(count + 1);
  // count가 0이면, 두 번 호출해도 1이 됨 (배치 처리)
}

// 함수형 업데이트로 해결:
setCount(prev => prev + 1);
setCount(prev => prev + 1); // 2가 됨
```

**setState 적용 과정:**
1. `setState` 호출 → 업데이트 큐에 추가
2. 이벤트 핸들러 종료 후 배치된 업데이트 처리
3. 새로운 상태로 컴포넌트 **리렌더링**
4. Virtual DOM Diffing
5. 실제 DOM 업데이트

**React 18 자동 배칭(Auto Batching):**
- React 17 이하: React 이벤트 핸들러 내에서만 배칭
- React 18: `setTimeout`, `fetch` 등 비동기 함수 안에서도 자동 배칭

</details>

---

## React 18과 이전의 차이

> React 18에서 추가된 주요 변경사항을 설명해주세요.

<details>
<summary>답변 보기</summary>

**1. 자동 배칭(Automatic Batching)**
- React 17: 이벤트 핸들러 내에서만 배칭
- React 18: 어디서든 자동 배칭 적용

**2. Concurrent Features**
- `startTransition`: 긴급하지 않은 업데이트를 낮은 우선순위로 처리
- `useDeferredValue`: 값 업데이트를 지연시켜 UI 응답성 유지

**3. Suspense 개선**
- SSR에서 Suspense 지원 (`renderToPipeableStream`)
- 컴포넌트 단위 스트리밍 가능

**4. 새로운 Hooks**
- `useId`: 서버-클라이언트 간 일관된 고유 ID 생성
- `useTransition`: 전환 중 상태 추적
- `useSyncExternalStore`: 외부 스토어 동기화

**5. `createRoot` API**
```js
// React 17
ReactDOM.render(<App />, document.getElementById('root'));

// React 18
ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```

</details>

---

## 좋은 컴포넌트란

> 본인이 생각하는 좋은 컴포넌트의 기준은 무엇인가요?

<details>
<summary>답변 보기</summary>

**1. 단일 책임 원칙(SRP):** 하나의 컴포넌트는 하나의 역할만 담당
**2. 재사용 가능:** 특정 비즈니스 로직에 강결합되지 않고 다양한 컨텍스트에서 사용 가능
**3. 예측 가능:** 같은 props를 받으면 항상 같은 결과 출력 (순수 함수)
**4. 접근성(a11y) 준수:** 스크린 리더, 키보드 네비게이션 지원
**5. 적절한 추상화:** 너무 많은 props(prop drilling), 너무 큰 컴포넌트 지양

</details>

---

## Key Props 사용 이유

> React에서 Key props를 사용하는 이유를 설명해주세요.

<details>
<summary>답변 보기</summary>

`key`는 React가 리스트에서 **어떤 아이템이 변경/추가/삭제됐는지 식별**하는 데 사용합니다.

**key가 없을 때의 문제:**
```jsx
// key가 없으면 React는 인덱스 기준으로 비교
// 리스트 중간에 아이템 추가/삭제 시 불필요한 리렌더링 발생
[A, B, C] → [A, X, B, C] // X 추가 시 B, C도 리렌더링
```

**인덱스를 key로 쓰면 안 되는 이유:**
- 아이템 순서가 바뀌면 key도 바뀌어 React가 잘못된 요소를 재사용
- 입력 컴포넌트의 경우 value가 엉켜버릴 수 있음

**올바른 key 사용:**
```jsx
items.map(item => <Item key={item.id} data={item} />)
```

</details>

---

## Flux와 MVC, CSR, SSR 구조

> React에서 Flux와 MVC 구조의 차이를 설명해주세요.

<details>
<summary>답변 보기</summary>

**MVC 패턴:**
- Model (데이터) ↔ Controller (로직) ↔ View (UI)
- **양방향 데이터 흐름** → 규모가 커지면 데이터 흐름 추적 어려움

**Flux 패턴 (Facebook이 React와 함께 제안):**
- **단방향 데이터 흐름**: Action → Dispatcher → Store → View
- View는 Action만 발생시킬 수 있고, 직접 Store를 수정 불가
- 예측 가능하고 디버깅 용이

**Redux:** Flux의 구현체, `Action → Reducer → Store → View`

</details>
