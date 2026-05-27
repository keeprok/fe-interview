# 🔧 JavaScript 핵심 개념

## 목차
- [var, let, const 차이](#var-let-const-차이)
- [호이스팅](#호이스팅)
- [스코프 (Scope)](#스코프-scope)
- [클로저 (Closure)](#클로저-closure)
- [this](#this)
- [일반함수와 화살표 함수 차이](#일반함수와-화살표-함수-차이)
- [forEach와 map 차이](#foreach와-map-차이)
- [콜백 함수](#콜백-함수)
- [가비지 컬렉터](#가비지-컬렉터)
- [얕은 복사와 깊은 복사](#얕은-복사와-깊은-복사)

---

## var, let, const 차이

> var, let, const의 차이점을 스코프, 호이스팅 관점에서 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | var | let | const |
|------|-----|-----|-------|
| 스코프 | 함수 스코프 | 블록 스코프 | 블록 스코프 |
| 재선언 | 가능 | 불가 | 불가 |
| 재할당 | 가능 | 가능 | 불가 |
| 호이스팅 | 선언+초기화(undefined) | 선언만 (TDZ) | 선언만 (TDZ) |
| 전역 객체 | window에 등록 | 등록 안됨 | 등록 안됨 |

**TDZ(Temporal Dead Zone):** `let`, `const`는 선언 전에 접근하면 `ReferenceError` 발생

</details>

---

## 호이스팅

> 호이스팅(Hoisting)이란 무엇인지 설명해주세요.

<details>
<summary>답변 보기</summary>

호이스팅은 JavaScript 엔진이 코드 실행 전 **선언을 최상단으로 끌어올리는 동작**입니다.

```js
console.log(a); // undefined (var는 선언+초기화 호이스팅)
var a = 10;

console.log(b); // ReferenceError (let은 TDZ)
let b = 20;

foo(); // 정상 실행 (함수 선언식은 전체 호이스팅)
function foo() { console.log('foo'); }

bar(); // TypeError: bar is not a function (함수 표현식은 변수만 호이스팅)
var bar = function() {};
```

**함수 선언식 vs 함수 표현식:**
- 함수 선언식: 전체가 호이스팅 → 선언 전에 호출 가능
- 함수 표현식: 변수만 호이스팅 → 선언 전에 호출 불가

</details>

---

## 스코프 (Scope)

> JavaScript의 스코프에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

**스코프**는 변수에 접근할 수 있는 유효 범위입니다.

**종류:**
- **전역 스코프**: 어디서든 접근 가능
- **함수 스코프** (var): 함수 내부에서만 접근 가능
- **블록 스코프** (let, const): `{}` 블록 내부에서만 접근 가능

**스코프 체인:** 변수를 찾을 때 현재 스코프 → 외부 스코프 순으로 탐색

```js
const outer = 'outer';
function foo() {
  const inner = 'inner';
  console.log(outer); // 스코프 체인으로 접근 가능
}
console.log(inner); // ReferenceError
```

**렉시컬 스코프:** JavaScript는 함수를 **정의한 위치**를 기준으로 스코프를 결정합니다 (호출 위치가 아님).

</details>

---

## 클로저 (Closure)

> 클로저(Closure)란 무엇이고 어떻게 활용하나요?

<details>
<summary>답변 보기</summary>

**클로저**는 함수가 자신이 선언된 시점의 외부 스코프 변수를 **기억하고 접근할 수 있는 기능**입니다.

```js
function counter() {
  let count = 0; // 외부 함수의 변수
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count,
  };
}

const c = counter();
c.increment(); // 1
c.increment(); // 2
c.getCount();  // 2 (count 변수는 외부에서 접근 불가하지만 클로저로 유지)
```

**활용 사례:**
- 데이터 은닉 (private 변수 흉내)
- 커링(Currying)
- React의 `useState`가 내부적으로 클로저 활용
- 이벤트 핸들러에서 외부 변수 참조

**주의:** 클로저가 참조하는 변수는 GC 대상에서 제외 → 메모리 누수 주의

</details>

---

## this

> JavaScript에서 this가 결정되는 방식을 설명해주세요.

<details>
<summary>답변 보기</summary>

`this`는 **함수가 호출되는 방식**에 따라 동적으로 결정됩니다.

| 호출 방식 | this |
|----------|------|
| 전역 스코프 | `window` (브라우저) / `global` (Node.js) |
| 일반 함수 호출 | `window` (strict mode: `undefined`) |
| 메서드 호출 | 해당 객체 |
| `new` 생성자 | 새로 생성된 인스턴스 |
| `call/apply/bind` | 첫 번째 인자로 지정한 객체 |
| 화살표 함수 | **정의된 시점의 외부 this** (동적 바인딩 없음) |

```js
const obj = {
  name: 'obj',
  regular: function() { console.log(this.name); }, // 'obj'
  arrow: () => { console.log(this.name); },        // undefined (전역 this)
};
```

</details>

---

## 일반함수와 화살표 함수 차이

> 일반 함수와 화살표 함수의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | 일반 함수 | 화살표 함수 |
|------|----------|-----------|
| `this` 바인딩 | 호출 방식에 따라 동적 결정 | 외부 스코프의 this 상속 (고정) |
| `arguments` 객체 | 있음 | 없음 (rest 파라미터 사용) |
| 생성자 함수 | 가능 (`new` 사용) | 불가 |
| `prototype` | 있음 | 없음 |
| 호이스팅 | 함수 선언식은 전체 호이스팅 | 불가 |

**React에서 화살표 함수 이벤트 핸들러:**
```js
class Component extends React.Component {
  // 화살표 함수로 this 바인딩 문제 해결
  handleClick = () => {
    console.log(this); // 컴포넌트 인스턴스
  }
}
```

</details>

---

## forEach와 map 차이

> forEach와 map의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | forEach | map |
|------|---------|-----|
| 반환값 | `undefined` | **새로운 배열** |
| 사용 목적 | 부수 효과(side effect) 수행 | 데이터 변환 |
| 체이닝 | 불가 | 가능 (filter, reduce 등) |

```js
const nums = [1, 2, 3];

// forEach: 반환값 없음
nums.forEach(n => console.log(n * 2));

// map: 새 배열 반환
const doubled = nums.map(n => n * 2); // [2, 4, 6]
```

**React에서:** JSX 렌더링에는 `map`을 사용 (반환값이 있어야 하기 때문)

</details>

---

## 콜백 함수

> 콜백 함수(Callback Function)란 무엇인지 설명해주세요.

<details>
<summary>답변 보기</summary>

**콜백 함수**는 다른 함수에 인자로 전달되어, 해당 함수 내부에서 나중에 호출되는 함수입니다.

```js
function fetchData(url, callback) {
  // 데이터 패칭 후...
  callback(data);
}

fetchData('/api/users', (data) => {
  console.log(data);
});
```

**콜백 지옥(Callback Hell):** 중첩된 콜백으로 인한 가독성 문제
```js
getData(function(a) {
  getMoreData(a, function(b) {
    getEvenMoreData(b, function(c) {
      // ... 콜백 지옥
    });
  });
});
```

이를 해결하기 위해 **Promise → async/await**가 등장했습니다.

</details>

---

## 가비지 컬렉터

> JavaScript의 가비지 컬렉터(Garbage Collector)에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

**가비지 컬렉터**는 더 이상 참조되지 않는 메모리를 자동으로 해제하는 메커니즘입니다.

**Mark-and-Sweep 알고리즘:**
1. **Mark**: 루트(전역 변수)에서 시작해 도달 가능한 모든 객체에 마크
2. **Sweep**: 마크되지 않은 객체(도달 불가능한 객체)를 메모리에서 제거

**메모리 누수 주의 상황:**
- 전역 변수 과도한 사용
- 제거하지 않은 이벤트 리스너
- 클로저가 불필요한 참조를 계속 유지
- 순환 참조

</details>

---

## 얕은 복사와 깊은 복사

> 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)의 차이를 설명해주세요.

<details>
<summary>답변 보기</summary>

**얕은 복사:** 객체의 1단계 프로퍼티만 복사 (중첩 객체는 참조를 공유)

```js
const original = { a: 1, b: { c: 2 } };
const shallow = { ...original }; // 얕은 복사

shallow.a = 99;     // original.a 영향 없음
shallow.b.c = 99;   // original.b.c도 99로 변경됨! (참조 공유)
```

**깊은 복사:** 중첩된 객체까지 모두 새로 복사

```js
// 방법 1: JSON 직렬화 (함수, undefined, Symbol 처리 불가)
const deep1 = JSON.parse(JSON.stringify(original));

// 방법 2: structuredClone (모던 브라우저 지원)
const deep2 = structuredClone(original);

// 방법 3: lodash cloneDeep
import cloneDeep from 'lodash/cloneDeep';
const deep3 = cloneDeep(original);
```

</details>
