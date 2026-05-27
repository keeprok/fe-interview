# 🧩 헤드리스 디자인 시스템 면접 대비

> Button, Dialog, Input, Select 컴포넌트를 직접 구현한 헤드리스 디자인 시스템 프로젝트를 기반으로 정리한 면접 질문들입니다.

## 우선순위 요약

| 우선순위 | 주제 | 관련 컴포넌트 | 파일 |
|---------|------|------------|------|
| ★★★ | Compound Component + Context 패턴 | Dialog, Select | [react-patterns.md](./react-patterns.md) |
| ★★★ | Controlled vs Uncontrolled | Select, Input | [react-patterns.md](./react-patterns.md) |
| ★★★ | forwardRef 사용 이유 | Button, Input | [react-patterns.md](./react-patterns.md) |
| ★★★ | Portal의 필요성과 이벤트 버블링 | Dialog, Select | [react-patterns.md](./react-patterns.md) |
| ★★★ | aria-disabled vs disabled | Button | [accessibility.md](./accessibility.md) |
| ★★★ | Focus Trap 구현 원리 | Dialog | [accessibility.md](./accessibility.md) |
| ★★★ | 헤드리스 컴포넌트란? 일반 컴포넌트와 차이 | 전체 | [headless-component.md](./headless-component.md) |
| ★★★ | Slot 패턴(asChild) vs as prop | Button | [headless-component.md](./headless-component.md) |
| ★★☆ | useId와 SSR Hydration mismatch | Input, Dialog | [react-patterns.md](./react-patterns.md) |
| ★★☆ | useMemo로 Context 최적화 | Select | [react-patterns.md](./react-patterns.md) |
| ★★☆ | Polymorphic 컴포넌트 타입 설계 | Button | [typescript-advanced.md](./typescript-advanced.md) |
| ★★☆ | WAI-ARIA 역할과 속성 | Dialog, Select | [accessibility.md](./accessibility.md) |
| ★★☆ | Roving tabindex 패턴 | Select | [accessibility.md](./accessibility.md) |
| ★☆☆ | Context 분리 최적화 (State/Dispatch) | Select | [react-patterns.md](./react-patterns.md) |
| ★☆☆ | cloneElement 주의점 | Slot | [headless-component.md](./headless-component.md) |

## 파일 목록

| 파일 | 내용 |
|------|------|
| [react-patterns.md](./react-patterns.md) | Compound Component, Context, forwardRef, Portal, Controlled/Uncontrolled, useMemo 최적화 |
| [accessibility.md](./accessibility.md) | aria-disabled, Focus Trap, WAI-ARIA, Roving tabindex |
| [typescript-advanced.md](./typescript-advanced.md) | Polymorphic 컴포넌트, Omit, 유틸리티 타입 |
| [headless-component.md](./headless-component.md) | 헤드리스 컴포넌트 개념, Slot 패턴, 스타일 분리 |

## 필수 암기 질문

**"Compound Component 패턴으로 Dialog/Select를 설계한 이유는 무엇인가요?"**

헤드리스 컴포넌트로 스타일을 외부에서 주입받으면서도, Dialog의 Trigger/Content/Close 같은 서브컴포넌트들이 `열림/닫힘 상태`를 공유해야 했기 때문입니다. Props drilling 없이 Context API로 상태를 공유하고, 사용자가 컴포넌트 내부 구조를 자유롭게 조합할 수 있도록 Compound Component 패턴을 선택했습니다.
