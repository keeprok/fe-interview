# 🥜 FE 기술 면접 대비 정리

프론트엔드 개발자 기술 면접을 위한 개념 정리 레포지토리입니다.
각 주제별로 질문과 답변이 `<details>` 태그로 정리되어 있어, 스스로 답변을 생각해본 뒤 확인할 수 있습니다.

---

## 📂 목차

| 카테고리 | 주제 |
|---------|------|
| [💻 CS](#-cs) | 브라우저, 네트워크, 자료구조, 디자인 패턴, Git |
| [🎨 HTML / CSS](#-html--css) | HTML, CSS, 이벤트, 접근성 |
| [🟨 JavaScript](#-javascript) | 핵심 개념, 비동기 |
| [⚛️ React](#️-react) | 핵심 개념, Hooks |
| [🔷 TypeScript](#-typescript) | 타입 시스템, 유틸리티 타입, 제네릭 |
| [▲ Next.js](#-nextjs) | 렌더링 방식, App Router, Hydration |
| [🧩 헤드리스 디자인 시스템](#-헤드리스-디자인-시스템) | Compound Component, 접근성, Polymorphic, Headless |

---

## 💻 CS

> 네트워크, 브라우저, 운영체제, 자료구조, 디자인 패턴

| 파일 | 주요 질문 |
|------|---------|
| [브라우저](./cs/browser.md) | 브라우저 동작 원리, CSR/SSR 차이, 쿠키/세션/로컬스토리지, Virtual DOM |
| [네트워크](./cs/network.md) | HTTP 메소드, HTTPS, CORS, TCP/UDP, RESTful API, AJAX |
| [자료구조 & OS](./cs/data-structure.md) | 스택/큐 차이, 프로세스/스레드 차이, 멀티스레딩 장단점 |
| [디자인 패턴 & 웹팩](./cs/design-pattern.md) | 디자인 패턴 종류, Presentational&Container, 웹팩 |
| [Git](./cs/git.md) | Merge/Squash/Rebase 전략, Git Flow |

---

## 🎨 HTML / CSS

> 마크업, 스타일링, 이벤트

| 파일 | 주요 질문 |
|------|---------|
| [HTML](./html-css/html.md) | Attribute/Property 차이, event.target vs currentTarget, 버블링/캡처링 |
| [CSS](./html-css/css.md) | 선택자 우선순위, 반응형 웹 3요소, Flex/Grid 차이, CSS Cascading, CSS-in-JS |

---

## 🟨 JavaScript

> ES6+, 비동기, 실행 컨텍스트

| 파일 | 주요 질문 |
|------|---------|
| [핵심 개념](./js/core.md) | var/let/const, 호이스팅, 스코프, 클로저, this, 화살표함수, forEach/map, 콜백, GC, 얕은/깊은복사 |
| [비동기](./js/async.md) | 이벤트 루프, Microtask Queue, Promise vs Async/Await, 비동기 동작 원리 |

---

## ⚛️ React

> 컴포넌트, Hooks, 렌더링 최적화

| 파일 | 주요 질문 |
|------|---------|
| [핵심 개념](./react/core.md) | 장단점, 라이프사이클, 원시/참조값, setState 동작, React 18 변경사항, Key props |
| [Hooks](./react/hooks.md) | useCallback/useMemo, useEffect/useLayoutEffect, useRef, useId, useTransition |

---

## 🔷 TypeScript

> 정적 타입, 유틸리티 타입, 제네릭

| 파일 | 주요 질문 |
|------|---------|
| [핵심 개념](./ts/core.md) | TS 사용 이유, type vs interface, Partial/Omit/Pick/Required, 제네릭, ComponentProps |

---

## ▲ Next.js

> SSR, SSG, ISR, App Router

| 파일 | 주요 질문 |
|------|---------|
| [핵심 개념](./next/core.md) | Next.js 사용 이유, SSR/SSG/ISR 차이, Hydration, App Router vs Pages Router |

---

## 🧩 헤드리스 디자인 시스템

> Button, Dialog, Input, Select를 직접 구현한 헤드리스 디자인 시스템 프로젝트 기반 면접 질문

| 파일 | 주요 질문 | 관련 컴포넌트 |
|------|---------|------------|
| [React 패턴](./design-system/react-patterns.md) | Compound Component, Context API, Controlled/Uncontrolled, forwardRef, Portal, useMemo 최적화 | Dialog, Select, Input, Button |
| [접근성 (a11y)](./design-system/accessibility.md) | aria-disabled vs disabled, Focus Trap, WAI-ARIA, aria-labelledby, Roving tabindex | Dialog, Button, Select |
| [TypeScript 심화](./design-system/typescript-advanced.md) | Polymorphic 컴포넌트, Omit 사용 이유, forwardRef+제네릭 | Button |
| [헤드리스 컴포넌트](./design-system/headless-component.md) | 헤드리스 컴포넌트 개념, Slot(asChild) vs as prop, cloneElement, 스타일 분리 이유 | 전체 |

### 면접 우선순위

| 우선순위 | 질문 |
|---------|------|
| ★★★ | Compound Component 패턴으로 Dialog/Select를 설계한 이유 |
| ★★★ | aria-disabled와 disabled의 차이 |
| ★★★ | Focus Trap이란 무엇이고 왜 필요한가 |
| ★★★ | Portal을 사용하는 이유 (overflow, z-index 문제) |
| ★★★ | forwardRef를 사용하는 이유 |
| ★★★ | Controlled vs Uncontrolled, 두 모드를 동시에 지원하는 방법 |
| ★★☆ | useId와 SSR Hydration mismatch |
| ★★☆ | Polymorphic 컴포넌트 타입 설계 |
| ★★☆ | Slot 패턴(asChild) vs as prop |

---

## 📝 학습 방법

1. 각 파일의 질문을 보고 `<details>` 를 열기 전 **스스로 답변을 먼저 구성**해봅니다
2. 답변을 확인 후 부족한 부분을 보충합니다
3. 특히 `★★★` 우선순위 질문은 **말로 유창하게 설명**할 수 있을 때까지 반복합니다
