# 🪝 React Hooks

## 목차
- [useCallback과 useMemo](#usecallback과-usememo)
- [useEffect와 useLayoutEffect의 차이](#useeffect와-uselayouteffect의-차이)
- [useRef](#useref)
- [useId (React 18)](#useid-react-18)
- [useTransition](#usetransition)

---

## useCallback과 useMemo

> useCallback과 useMemo의 차이점과 언제 사용해야 하는지 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | useMemo | useCallback |
|------|---------|-------------|
| 메모이제이션 대상 | **계산된 값** | **함수** |
| 반환 | 계산 결과 | 메모이제이션된 함수 |

```js
// useMemo: 비용이 큰 계산 결과를 캐싱
const filteredList = useMemo(
  () => items.filter(item => item.active),
  [items]
);

// useCallback: 함수 참조를 안정적으로 유지
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);
```

**언제 사용해야 하나요?**
- `useMemo`: 렌더링마다 실행되는 **비용이 큰 계산**
- `useCallback`: 자식 컴포넌트에 전달하는 함수 (React.memo와 함께)
- **Context value 객체** 메모이제이션 (`useMemo(() => ({ state, dispatch }), [state, dispatch])`)

**주의:** 무조건 쓰는 게 좋지 않습니다. 메모이제이션 자체도 비용이 있으므로, 실제 성능 문제가 있을 때 사용합니다.

</details>

---

## useEffect와 useLayoutEffect의 차이

> useEffect와 useLayoutEffect의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | useEffect | useLayoutEffect |
|------|-----------|----------------|
| 실행 시점 | 브라우저 **페인트 이후** | DOM 업데이트 후, **페인트 이전** |
| 동기/비동기 | 비동기 | 동기 (페인트 블로킹) |
| 사용 사례 | 데이터 패칭, 이벤트 구독 | DOM 측정, 레이아웃 관련 작업 |

```js
// DOM 측정이 필요할 때 useLayoutEffect 사용
useLayoutEffect(() => {
  const rect = elementRef.current.getBoundingClientRect();
  // 페인트 전에 DOM 크기/위치 계산 → 깜빡임 없음
}, []);
```

**SSR 주의:** `useLayoutEffect`는 서버에서 실행되지 않아 경고 발생. SSR 환경에서는 `useEffect` 사용.

</details>

---

## useRef

> useRef는 언제 사용하고 useState와 어떤 차이가 있나요?

<details>
<summary>답변 보기</summary>

**useRef의 두 가지 사용 목적:**

**1. DOM 요소에 직접 접근:**
```js
const inputRef = useRef(null);

const focusInput = () => {
  inputRef.current.focus();
};

return <input ref={inputRef} />;
```

**2. 렌더링 없이 값 유지:**
```js
const countRef = useRef(0);
countRef.current++; // 리렌더링 발생하지 않음
```

| 구분 | useState | useRef |
|------|---------|--------|
| 리렌더링 | 값 변경 시 리렌더링 | 리렌더링 없음 |
| 값 접근 | `state` | `ref.current` |
| 용도 | UI에 반영되는 상태 | DOM 접근, 렌더 사이클 외 값 |

</details>

---

## useId (React 18)

> useId 훅은 왜 등장했나요?

<details>
<summary>답변 보기</summary>

`useId`는 서버-클라이언트 간 **Hydration 불일치 문제**를 해결하기 위해 React 18에서 등장했습니다.

**문제:**
```js
// Math.random()이나 counter를 사용하면
// 서버와 클라이언트에서 다른 값 생성 → hydration mismatch
const id = Math.random(); // 서버: 0.123, 클라이언트: 0.456
```

**해결:**
```js
const id = useId(); // 서버와 클라이언트에서 동일한 값 보장
// ':r0:', ':r1:' 형태로 생성

return (
  <>
    <label htmlFor={id}>이름</label>
    <input id={id} />
  </>
);
```

</details>

---

## useTransition

> useTransition 훅은 무엇이고 언제 사용하나요?

<details>
<summary>답변 보기</summary>

`useTransition`은 **우선순위가 낮은 상태 업데이트**를 표시하여 UI 응답성을 유지합니다.

```js
const [isPending, startTransition] = useTransition();

function handleSearch(query) {
  // 긴급: 입력값은 즉시 업데이트
  setInputValue(query);

  // 비긴급: 필터링 결과는 지연 가능
  startTransition(() => {
    setFilteredItems(items.filter(item => item.includes(query)));
  });
}

return (
  <>
    <input onChange={e => handleSearch(e.target.value)} />
    {isPending ? <Spinner /> : <ItemList items={filteredItems} />}
  </>
);
```

</details>
