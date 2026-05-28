# ⚛️ React 패턴

> 관련 컴포넌트: Dialog, Select, Input, Button

---

## Context API / Compound Component 패턴

- Context API를 사용하는 이유가 뭔가요? Props drilling과 비교해서 설명해주세요.
- `createContext`의 기본값(defaultValue)과 Provider의 value의 차이점은 무엇인가요?
- Compound Component 패턴이란 무엇이고, 어떤 장점이 있나요?
- Context를 사용할 때 리렌더링 문제가 생길 수 있는데, 어떻게 최적화하시나요?

---

## useMemo / 최적화

- `useMemo`는 언제 써야 하나요? 항상 쓰는 게 좋은가요?
- Context value에 인라인 객체를 그대로 넣으면 어떤 문제가 생기나요?
- State Context와 Dispatch Context를 분리하면 어떤 이점이 있나요?

---

## forwardRef

- `forwardRef`는 왜 사용하나요? 언제 필요한가요?
- `forwardRef`를 쓸 때 TypeScript 타입을 어떻게 정의하셨나요?
- `displayName`은 왜 설정하나요?

---

## useEffect / useRef

- `useEffect`와 `useLayoutEffect`의 차이점은 무엇인가요?
- `useEffect`의 의존성 배열(dependency array)에서 함수를 넣을 때 주의할 점은 무엇인가요?
- `useRef`로 DOM에 직접 접근하는 게 React 철학과 충돌하지 않나요?

---

## useId (React 18)

- `useId` 훅은 왜 등장했나요? `Math.random()`이나 counter로 id를 만들면 안 되나요?

---

## Controlled vs Uncontrolled

- Controlled 컴포넌트와 Uncontrolled 컴포넌트의 차이점을 설명해주세요.
- 두 모드를 동시에 지원하는 컴포넌트를 어떻게 설계하시겠어요?
- react-hook-form 같은 라이브러리와 헤드리스 컴포넌트를 어떻게 연결하나요?

---

## createPortal

- Portal을 왜 사용하나요? 그냥 일반 렌더링하면 안 되나요?
- Portal로 렌더된 컴포넌트의 이벤트 버블링은 어떻게 동작하나요?
