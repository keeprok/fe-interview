# 🎯 디자인 시스템 실전 면접 예상 질문

> 직접 구현한 헤드리스 디자인 시스템(Button, Dialog, Input, Select) 기반 포트폴리오 면접 예상 질문 목록입니다.
> 대본은 따로 준비, 여기선 질문만 정리합니다.

---

## 1. 헤드리스 / Compound Component 패턴

**메인 질문**
- 디자인 시스템을 굳이 Headless와 Compound 패턴으로 쪼개서 만든 이유가 뭔가요? 코드가 너무 복잡해지지 않나요?

**꼬리 질문**
- Compound Component 패턴 말고 동일한 문제를 해결할 수 있는 다른 패턴이 있다면 무엇인가요? 왜 그걸 선택하지 않았나요?
- Radix UI, Headless UI 같은 기존 라이브러리를 쓰지 않고 직접 만든 이유는 뭔가요?

---

## 2. 다형성(Polymorphic) & asChild 슬롯 패턴

**메인 질문**
- Button 컴포넌트에서 `as`나 `asChild` 패턴을 쓰셨는데, 왜 이렇게 구현했나요?

**꼬리 질문**
- `as` prop 방식과 `asChild(Slot)` 방식의 차이점이 뭔가요? 어떤 상황에서 각각 더 유리한가요?
- `asChild`를 구현할 때 `React.cloneElement`를 쓰셨나요? 사용 시 주의할 점은 무엇인가요?

---

## 3. Controlled vs Uncontrolled 동시 지원

**메인 질문**
- Select나 Input에서 제어/비제어 상태를 동시에 지원하도록 하셨는데, 어떻게 구현했고 왜 그랬나요?

**꼬리 질문**
- 컴포넌트가 렌더링 중에 controlled에서 uncontrolled로 전환되면 React가 경고를 띄우는데, 이 문제를 어떻게 방어하셨나요?
- `react-hook-form`의 `Controller`와 연결할 때 어떤 방식으로 붙이셨나요?

---

## 4. Context API 렌더링 최적화

**메인 질문**
- Context API를 쓰면 구독하는 자식들이 전부 리렌더링되는 이슈가 있는데, 최적화는 어떻게 하셨나요?

**꼬리 질문**
- `useMemo`로 value를 캐싱하는 것만으로 충분한가요? 한계가 있다면 다음 단계로 어떤 방법을 쓸 수 있나요?
- State Context와 Dispatch Context를 분리하면 왜 리렌더링이 줄어드나요?

---

## 5. Focus Trap (Dialog) & Roving Tabindex (Select)

**메인 질문**
- 직접 키보드 네비게이션을 짰다고 하셨는데, 구체적으로 어떤 고민을 하셨나요?

**꼬리 질문**
- Dialog가 닫힐 때 포커스를 어디로 돌려보내셨나요? 그 이유는 무엇인가요?
- 모달 안에 또 다른 모달이 중첩되는 경우, Focus Trap은 어떻게 동작해야 하나요?

---

## 6. 웹 접근성 (WAI-ARIA)

**메인 질문**
- 헤드리스면 UI가 없는데, 접근성 처리는 뭘 어떻게 했다는 거죠?

**꼬리 질문**
- `aria-modal="true"`와 `role="dialog"`는 같이 써야 하나요? 각각 어떤 역할을 하나요?
- 구현한 컴포넌트의 접근성을 어떻게 검증하셨나요? (테스트 방법)

---

## 7. createPortal

**메인 질문**
- Dialog에서 `createPortal`을 쓴 이유는 뭔가요?

**꼬리 질문**
- Portal로 렌더된 컴포넌트에서 발생한 이벤트는 DOM 트리 기준으로 버블링되나요, React 트리 기준으로 버블링되나요?
- Portal을 쓸 때 발생할 수 있는 단점이나 주의사항이 있다면 무엇인가요?

---

## 8. forwardRef와 TypeScript 제네릭

**메인 질문**
- 모든 컴포넌트에 `forwardRef`를 감싼 이유가 있나요?

**꼬리 질문**
- `forwardRef`와 제네릭을 함께 쓸 때 TypeScript 타입 추론이 깨지는 문제가 있는데, 어떻게 해결하셨나요?
- `useImperativeHandle`은 언제 사용하고, `forwardRef`만 쓰는 것과 어떻게 다른가요?

---

## 9. 이벤트 핸들러 합성 (Event Composition)

**메인 질문**
- 컴포넌트 내부 로직과 외부에서 Props로 넘겨준 `onClick`이 겹칠 때 어떻게 처리했나요?

**꼬리 질문**
- 외부 핸들러에서 `e.preventDefault()`를 호출했을 때 내부 동작도 막아야 하는 경우는 어떻게 처리하셨나요?
- 이벤트 합성 로직을 매번 인라인으로 작성하지 않고 재사용 가능하게 추상화한다면 어떻게 하시겠어요?
