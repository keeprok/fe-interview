# 🧩 헤드리스 디자인 시스템 면접 대비

> Button, Dialog, Input, Select 컴포넌트를 직접 구현한 헤드리스 디자인 시스템 프로젝트 기반 예상 질문 목록입니다.
> 질문만 정리되어 있습니다. 스스로 답변 후 필요하면 답변 추가 예정.

---

## 우선순위

| 우선순위 | 주제 | 관련 컴포넌트 | 파일 |
|---------|------|------------|------|
| ★★★ | Compound Component + Context 패턴 | Dialog, Select | [react-patterns.md](./react-patterns.md) |
| ★★★ | Controlled vs Uncontrolled | Select, Input | [react-patterns.md](./react-patterns.md) |
| ★★★ | forwardRef 사용 이유 | Button, Input | [react-patterns.md](./react-patterns.md) |
| ★★★ | Portal의 필요성과 이벤트 버블링 | Dialog, Select | [react-patterns.md](./react-patterns.md) |
| ★★★ | aria-disabled vs disabled | Button | [accessibility.md](./accessibility.md) |
| ★★★ | Focus Trap 구현 원리 | Dialog | [accessibility.md](./accessibility.md) |
| ★★☆ | useId와 SSR hydration mismatch | Input, Dialog | [react-patterns.md](./react-patterns.md) |
| ★★☆ | useMemo로 Context 최적화 | Select | [react-patterns.md](./react-patterns.md) |
| ★★☆ | Polymorphic 컴포넌트 타입 설계 | Button | [typescript-advanced.md](./typescript-advanced.md) |
| ★★☆ | WAI-ARIA role / aria 속성 | Dialog, Select | [accessibility.md](./accessibility.md) |
| ★★☆ | Slot 패턴(asChild) vs as prop | Button | [headless-component.md](./headless-component.md) |
| ★☆☆ | Context 분리 최적화 (State/Dispatch) | Select | [react-patterns.md](./react-patterns.md) |
| ★☆☆ | cloneElement 주의점 | Slot | [headless-component.md](./headless-component.md) |

---

## 파일 목록

| 파일 | 내용 |
|------|------|
| [react-patterns.md](./react-patterns.md) | Context, Compound Component, forwardRef, Portal, Controlled/Uncontrolled, useMemo |
| [accessibility.md](./accessibility.md) | aria 속성, WAI-ARIA role, Focus Trap, Roving tabindex |
| [typescript-advanced.md](./typescript-advanced.md) | Polymorphic 컴포넌트, 유틸리티 타입 |
| [headless-component.md](./headless-component.md) | 헤드리스 컴포넌트 개념, Slot 패턴 |
