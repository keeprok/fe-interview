# ⚛️ 헤드리스 디자인 시스템 - React 패턴

> 우선순위: ★★★ (면접에서 반드시 나올 질문)

## 목차
- [Compound Component + Context 패턴](#compound-component--context-패턴)
- [Context API vs Props Drilling](#context-api-vs-props-drilling)
- [Controlled vs Uncontrolled 컴포넌트](#controlled-vs-uncontrolled-컴포넌트)
- [forwardRef](#forwardref)
- [createPortal](#createportal)
- [useMemo로 Context 최적화](#usememo로-context-최적화)

---

## Compound Component + Context 패턴

> ★★★ Compound Component 패턴이란 무엇이고, 어떤 장점이 있나요?

<details>
<summary>답변 보기</summary>

**Compound Component 패턴**은 여러 컴포넌트가 **암묵적으로 상태를 공유**하면서 함께 동작하는 설계 패턴입니다.

```tsx
// 사용 예시
<Dialog>
  <Dialog.Trigger>열기</Dialog.Trigger>
  <Dialog.Content>
    <Dialog.Title>제목</Dialog.Title>
    <Dialog.Close>닫기</Dialog.Close>
  </Dialog.Content>
</Dialog>

// 내부 구현: Context로 상태 공유
const DialogContext = createContext<DialogContextValue | null>(null);

function Dialog({ children }: { children: React.ReactNode }) {
  const [open, setOpen] = useState(false);
  const contextValue = useMemo(() => ({ open, setOpen }), [open]);
  return (
    <DialogContext.Provider value={contextValue}>
      {children}
    </DialogContext.Provider>
  );
}

Dialog.Trigger = function Trigger({ children }) {
  const { setOpen } = useDialogContext();
  return <button onClick={() => setOpen(true)}>{children}</button>;
};
```

**장점:**
1. **유연한 구조**: 사용자가 컴포넌트 내부 구조를 자유롭게 조합 가능
2. **Props 전달 없음**: Context를 통해 암묵적으로 상태 공유
3. **관심사 분리**: 각 서브컴포넌트가 자신의 역할만 담당
4. **재사용성**: 내부 구조가 고정되지 않아 다양한 레이아웃에 적용 가능

**"Dialog/Select를 Compound Component로 설계한 이유"** → 헤드리스 컴포넌트의 핵심 설계 결정입니다. 스타일을 외부에서 주입받으면서도, 내부 상태(열림/닫힘, 선택값)를 서브컴포넌트들이 공유해야 하기 때문입니다.

</details>

---

## Context API vs Props Drilling

> Context API를 사용하는 이유가 뭔가요? Props drilling과 비교해서 설명해주세요.

<details>
<summary>답변 보기</summary>

**Props Drilling 문제:**
```tsx
// A → B → C → D로 props를 전달해야 할 때
<A data={data}>
  <B data={data}>  {/* B는 data를 사용하지 않지만 전달만 함 */}
    <C data={data}> {/* C도 전달만 함 */}
      <D data={data} /> {/* 실제 사용처 */}
    </C>
  </B>
</A>
```

**문제점:**
- 중간 컴포넌트들이 불필요한 props를 받아야 함
- 컴포넌트 재사용성 감소
- props 이름 변경 시 전체 경로 수정 필요

**Context API 해결:**
```tsx
const DataContext = createContext(null);

function App() {
  return (
    <DataContext.Provider value={data}>
      <B /> {/* data props 불필요 */}
    </DataContext.Provider>
  );
}

function D() {
  const data = useContext(DataContext); // 바로 접근
  return <div>{data}</div>;
}
```

**`createContext`의 `defaultValue` vs `Provider`의 `value` 차이:**
- `defaultValue`: Provider 없이 useContext를 사용할 때 사용되는 값
- `Provider value`: 실제 사용되는 값. Provider가 있으면 defaultValue는 무시됨

</details>

---

## Controlled vs Uncontrolled 컴포넌트

> ★★★ Controlled 컴포넌트와 Uncontrolled 컴포넌트의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | Controlled | Uncontrolled |
|------|-----------|-------------|
| 상태 관리 | React state로 관리 | DOM이 내부적으로 관리 |
| 값 접근 | `value` prop + `onChange` | `ref` |
| 실시간 검증 | 쉬움 | 어려움 |
| 예측 가능성 | 높음 | 낮음 |

```tsx
// Controlled
function ControlledInput() {
  const [value, setValue] = useState('');
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}

// Uncontrolled
function UncontrolledInput() {
  const ref = useRef<HTMLInputElement>(null);
  const handleSubmit = () => console.log(ref.current?.value);
  return <input ref={ref} />;
}
```

**두 모드를 동시에 지원하는 컴포넌트 설계 (Select 컴포넌트):**
```tsx
function Select({ value: controlledValue, defaultValue, onChange }) {
  const [internalValue, setInternalValue] = useState(defaultValue);

  // controlledValue가 있으면 Controlled, 없으면 Uncontrolled
  const isControlled = controlledValue !== undefined;
  const value = isControlled ? controlledValue : internalValue;

  function handleChange(newValue) {
    if (!isControlled) setInternalValue(newValue);
    onChange?.(newValue);
  }
}
```

**react-hook-form 연결:** `Controller` 컴포넌트 또는 `register`를 통해 헤드리스 컴포넌트를 form에 통합할 수 있습니다.

</details>

---

## forwardRef

> ★★★ forwardRef는 왜 사용하나요? 언제 필요한가요?

<details>
<summary>답변 보기</summary>

`forwardRef`는 **부모 컴포넌트가 자식 컴포넌트 내부의 DOM 요소에 ref를 전달**할 수 있게 합니다.

**왜 필요한가?**
- React 컴포넌트는 기본적으로 `ref` prop을 받을 수 없음
- 디자인 시스템의 Input, Button 같은 래퍼 컴포넌트에서 DOM 접근이 필요할 때

```tsx
// forwardRef 없이: ref가 DOM에 전달되지 않음
function Input(props) {
  return <input {...props} />; // ref 전달 불가
}

// forwardRef 사용
const Input = React.forwardRef<HTMLInputElement, InputProps>(
  ({ className, ...props }, ref) => (
    <input ref={ref} className={className} {...props} />
  )
);

// 사용처에서 DOM에 직접 접근 가능
const inputRef = useRef<HTMLInputElement>(null);
<Input ref={inputRef} />
inputRef.current?.focus();
```

**TypeScript에서 forwardRef 타입 정의:**
```tsx
// 제네릭: <DOM 타입, Props 타입>
React.forwardRef<HTMLButtonElement, ButtonProps>((props, ref) => ...)
```

**displayName 설정 이유:**
```tsx
Input.displayName = 'Input'; // React DevTools에서 컴포넌트 이름 표시
```

</details>

---

## createPortal

> ★★★ Portal을 왜 사용하나요? 그냥 일반 렌더링하면 안 되나요?

<details>
<summary>답변 보기</summary>

**Portal**은 컴포넌트를 **DOM 계층 구조 밖의 다른 노드**에 렌더링합니다.

**왜 사용하나요?**
1. **CSS `overflow: hidden` 문제**: 부모에 `overflow: hidden`이 있으면 Dialog/Dropdown이 잘림
2. **z-index 스태킹 컨텍스트**: 부모의 z-index에 종속되어 레이어 관리 어려움

```tsx
function Dialog({ children, open }) {
  if (!open) return null;
  return createPortal(
    <div className="dialog-overlay">
      {children}
    </div>,
    document.body // body에 직접 렌더링
  );
}
```

**Portal의 이벤트 버블링:**
```
DOM 트리:    body > div#portal > Dialog
React 트리:  App > Parent > Dialog
```
- **DOM 트리** 기준이 아닌 **React 트리** 기준으로 이벤트가 버블링됨
- `Parent`에 onClick 핸들러가 있으면 Portal 안의 Dialog 클릭 시에도 발생

</details>

---

## useMemo로 Context 최적화

> ★★☆ Context value에 인라인 객체를 그대로 넣으면 어떤 문제가 생기나요?

<details>
<summary>답변 보기</summary>

```tsx
// 문제: 매 렌더마다 새로운 객체 참조 생성
function Select({ children }) {
  const [value, setValue] = useState('');

  return (
    // 렌더마다 새 객체 → Context 구독 컴포넌트 전체 리렌더링
    <SelectContext.Provider value={{ value, setValue }}>
      {children}
    </SelectContext.Provider>
  );
}

// 해결: useMemo로 참조 안정화
function Select({ children }) {
  const [value, setValue] = useState('');

  const contextValue = useMemo(
    () => ({ value, setValue }),
    [value] // value가 바뀔 때만 새 객체 생성
  );

  return (
    <SelectContext.Provider value={contextValue}>
      {children}
    </SelectContext.Provider>
  );
}
```

**더 나아간 최적화 - State/Dispatch Context 분리:**
```tsx
// 상태와 업데이트 함수를 분리하면
// 업데이트 함수만 쓰는 컴포넌트는 상태 변경 시 리렌더링 안됨
const SelectStateContext = createContext(null);   // { value }
const SelectDispatchContext = createContext(null); // { setValue }
```

</details>
