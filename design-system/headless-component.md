# 🧩 헤드리스 컴포넌트 설계

> 우선순위: ★★★ (포트폴리오 면접에서 반드시 나옴)

## 목차
- [헤드리스 컴포넌트란](#헤드리스-컴포넌트란)
- [Slot 패턴 (asChild)](#slot-패턴-aschild)
- [cloneElement 사용 시 주의점](#cloneelement-사용-시-주의점)
- [스타일을 컴포넌트 내부에 넣지 않은 이유](#스타일을-컴포넌트-내부에-넣지-않은-이유)

---

## 헤드리스 컴포넌트란

> ★★★ 헤드리스(Headless) 컴포넌트란 무엇인가요? 일반 컴포넌트와 어떻게 다른가요?

<details>
<summary>답변 보기</summary>

**헤드리스 컴포넌트**는 **로직과 상태만 제공하고, UI(스타일)는 전혀 포함하지 않는** 컴포넌트입니다.

| 구분 | 일반 컴포넌트 | 헤드리스 컴포넌트 |
|------|------------|----------------|
| 스타일 | 내장 | 없음 (외부에서 주입) |
| 재사용성 | 스타일이 맞아야 재사용 가능 | 어느 디자인에서든 사용 가능 |
| 커스터마이징 | 제한적 | 완전 자유 |
| 관심사 | 로직 + UI 혼재 | 로직만 담당 |

**예시 비교:**
```tsx
// 일반 컴포넌트: 스타일이 고정됨
<MyDialog className="override-style"> {/* 스타일 덮어쓰기 어려움 */}

// 헤드리스 컴포넌트: 스타일 완전 자유
<Dialog>
  <Dialog.Trigger className="my-button-style">열기</Dialog.Trigger>
  <Dialog.Content className="my-modal-style">
    <Dialog.Title className="my-title-style">제목</Dialog.Title>
  </Dialog.Content>
</Dialog>
```

**대표 라이브러리:** Radix UI, Headless UI, React Aria, Base UI

**프로젝트에서 헤드리스로 만든 이유:**
- 같은 컴포넌트가 여러 디자인 시스템(다크모드, 브랜드별)에서 재사용되어야 했기 때문
- 동작(접근성, 키보드 네비게이션, 상태관리)은 공통, 스타일은 각자 정의

</details>

---

## Slot 패턴 (asChild)

> ★★★ Slot 패턴(asChild)을 왜 사용했나요? as prop과 어떻게 다른가요?

<details>
<summary>답변 보기</summary>

**`as` prop 방식:**
```tsx
// 렌더링할 요소 타입을 변경하지만, 자식의 타입 정보는 잃음
<Button as="a" href="/about">링크</Button>
// 내부: React.createElement('a', { href: '/about', ...buttonProps })
```

**`asChild` (Slot) 패턴:**
```tsx
// 자식 컴포넌트의 타입, props, ref를 완전히 보존하면서 병합
<Button asChild>
  <a href="/about">링크</a> {/* a 태그의 타입 정보가 완전히 유지됨 */}
</Button>
```

**차이점:**
| 구분 | `as` prop | `asChild` (Slot) |
|------|---------|----------------|
| 자식 타입 보존 | X | O |
| TypeScript 지원 | 부분적 | 완전 |
| 사용 편의성 | 간단 | 약간 복잡 |
| Radix UI 방식 | X | O |

**Slot 구현:**
```tsx
function Slot({ children, ...slotProps }: { children: React.ReactElement }) {
  // 자식의 props와 Slot의 props를 병합
  return React.cloneElement(children, {
    ...slotProps,
    ...children.props, // 자식 props가 우선
    // 이벤트 핸들러는 두 개를 모두 호출
    onClick: composeEventHandlers(slotProps.onClick, children.props.onClick),
  });
}

// 사용
function Button({ asChild, children, ...props }) {
  if (asChild) {
    return <Slot {...props}>{children}</Slot>;
  }
  return <button {...props}>{children}</button>;
}
```

</details>

---

## cloneElement 사용 시 주의점

> cloneElement를 쓸 때 주의할 점은 무엇인가요?

<details>
<summary>답변 보기</summary>

`React.cloneElement`는 기존 React 요소를 복제하고 props를 병합합니다.

**주의점:**

**1. 자식이 단일 React 요소여야 함:**
```tsx
// 문제: 문자열, 배열, null 등은 처리 불가
<Slot>텍스트</Slot>       // 오류
<Slot>{[<div />, <div />]}</Slot> // 오류
<Slot><div /></Slot>      // 정상
```

**2. ref 병합 필요:**
```tsx
// cloneElement는 ref를 자동 병합하지 않음
React.cloneElement(child, props, ...children);
// ref는 별도로 mergeRefs 유틸로 처리 필요
```

**3. props 우선순위 명시:**
```tsx
// 어느 쪽 props가 우선인지 명확히 해야 함
React.cloneElement(child, {
  ...slotProps,  // Slot props
  ...child.props // 자식 props가 덮어씀 (또는 반대로)
});
```

**4. React 공식 문서에서 권장하지 않음:**
- 대안: render props, 합성(composition), 컨텍스트

</details>

---

## 스타일을 컴포넌트 내부에 넣지 않은 이유

> 디자인 시스템에서 스타일을 컴포넌트 내부에 넣지 않은 이유가 무엇인가요?

<details>
<summary>답변 보기</summary>

**헤드리스 컴포넌트에서 스타일을 분리한 이유:**

**1. 재사용성 극대화:**
- 같은 Dialog 컴포넌트를 light theme / dark theme / branded theme 등 다양한 시각적 표현에 재사용
- 스타일이 내장되면 매번 오버라이드해야 해서 CSS 명시도 전쟁 발생

**2. 관심사 분리:**
- 컴포넌트는 **동작**(상태, 접근성, 키보드 핸들링)만 책임
- **시각적 표현**은 사용하는 측에서 책임

**3. 번들 크기:**
- 스타일을 사용하지 않는 프로젝트에서도 스타일 코드가 포함되는 낭비 방지

**4. CSS 방법론 자유:**
```tsx
// Tailwind 사용자
<Dialog.Content className="fixed top-1/2 bg-white rounded-lg shadow-xl">

// CSS Modules 사용자
<Dialog.Content className={styles.dialogContent}>

// styled-components 사용자
<Dialog.Content as={StyledContent}>
```

**트레이드오프:**
- 사용 편의성 감소 (매번 스타일 직접 작성 필요)
- 이를 보완하기 위해 스타일이 적용된 "styled" 래퍼 컴포넌트를 별도로 제공하는 전략도 있음

</details>
