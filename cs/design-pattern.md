# 🏗️ 디자인 패턴 & 웹팩

## 목차
- [디자인 패턴이란](#디자인-패턴이란)
- [Presentational & Container 패턴](#presentational--container-패턴)
- [웹팩 (Webpack)](#웹팩-webpack)

---

## 디자인 패턴이란

> 자주 사용하는 디자인 패턴의 종류와 특징을 설명해주세요.

<details>
<summary>답변 보기</summary>

디자인 패턴은 소프트웨어 설계 시 자주 발생하는 문제에 대한 **재사용 가능한 해결책**입니다.

**생성 패턴:**
- **싱글톤(Singleton)**: 클래스 인스턴스를 하나만 생성
- **팩토리(Factory)**: 객체 생성 로직을 분리

**구조 패턴:**
- **어댑터(Adapter)**: 호환되지 않는 인터페이스를 연결
- **컴포지트(Composite)**: 트리 구조로 객체 구성

**행위 패턴:**
- **옵저버(Observer)**: 상태 변화 시 구독자에게 알림 (React의 useState와 유사)
- **전략(Strategy)**: 알고리즘을 캡슐화하여 교체 가능

</details>

---

## Presentational & Container 패턴

> Presentational & Container 디자인 패턴에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

React에서 관심사 분리를 위한 컴포넌트 분리 패턴입니다.

| 구분 | Presentational (View) | Container (Logic) |
|------|----------------------|------------------|
| 역할 | UI 렌더링 | 비즈니스 로직, 데이터 처리 |
| 상태 | 거의 없음 (UI 상태 제외) | 있음 |
| props | 데이터와 콜백 받음 | 데이터 패칭, 상태 관리 |
| 재사용성 | 높음 | 낮음 |

**현재는?** React Hooks의 등장으로 커스텀 훅으로 로직 분리가 더 선호됩니다. 하지만 **헤드리스 컴포넌트 패턴**은 이 아이디어의 연장선입니다.

</details>

---

## 웹팩 (Webpack)

> 웹팩(Webpack)이란 무엇이고 왜 사용하나요?

<details>
<summary>답변 보기</summary>

**Webpack**은 JavaScript 애플리케이션을 위한 **모듈 번들러**입니다.

**왜 사용하나요?**
- 수많은 JS, CSS, 이미지 파일을 하나(또는 소수)의 번들로 묶어 **HTTP 요청 수 감소**
- `import/export` 모듈 시스템을 브라우저가 이해할 수 있게 변환
- **Tree Shaking**: 사용하지 않는 코드 제거
- **코드 스플리팅**: 필요한 시점에 필요한 코드만 로드

**핵심 개념:**
- **Entry**: 번들링 시작점
- **Output**: 번들 출력 위치
- **Loader**: JS 외의 파일(CSS, 이미지 등) 처리 (ex: `babel-loader`, `css-loader`)
- **Plugin**: 번들 최적화, 환경변수 주입 등 (ex: `HtmlWebpackPlugin`)

</details>
