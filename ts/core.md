# 🔷 TypeScript

## 목차
- [TypeScript를 사용하는 이유](#typescript를-사용하는-이유)
- [타입(type)과 인터페이스(interface)의 차이](#타입type과-인터페이스interface의-차이)
- [유틸리티 타입](#유틸리티-타입)
- [제네릭 (Generics)](#제네릭-generics)
- [React.ComponentPropsWithoutRef vs React.ComponentPropsWithRef](#reactcomponentpropswithoutref-vs-reactcomponentpropswithref)

---

## TypeScript를 사용하는 이유

> JavaScript 대신 TypeScript를 사용하는 이유와 장단점을 설명해주세요.

<details>
<summary>답변 보기</summary>

**장점:**
- **정적 타입 검사**: 런타임 오류를 컴파일 타임에 발견
- **IDE 자동완성 및 IntelliSense** 향상
- **코드 가독성 향상**: 타입이 곧 문서 역할
- **리팩토링 안전성**: 타입 오류로 영향 범위 파악 가능
- **팀 협업**: 인터페이스 계약으로 명확한 소통

**단점:**
- 초기 설정 및 학습 비용
- 타입 정의 작성으로 코드량 증가
- 컴파일 시간 추가 발생
- 과도한 타입 복잡도 야기 가능

</details>

---

## 타입(type)과 인터페이스(interface)의 차이

> TypeScript에서 type과 interface의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | type | interface |
|------|------|-----------|
| 선언 병합 | 불가 | 가능 (같은 이름으로 확장) |
| 확장 | `&` (교차 타입) | `extends` |
| 원시 타입 별칭 | 가능 | 불가 |
| 유니온 타입 | 가능 | 불가 |
| 계산된 프로퍼티 | 가능 | 제한적 |

```ts
// 선언 병합 (interface만 가능)
interface User { name: string; }
interface User { age: number; } // name + age 둘 다 가짐

// 유니온 (type만 가능)
type Status = 'active' | 'inactive';

// 확장
type A = { x: number } & { y: number }; // type
interface B extends SomeInterface { z: number; } // interface
```

**권장:** 라이브러리 공개 API에는 `interface` (선언 병합 활용), 내부 구현에는 `type`

</details>

---

## 유틸리티 타입

> TypeScript의 주요 유틸리티 타입을 설명해주세요.

<details>
<summary>답변 보기</summary>

```ts
interface User {
  id: number;
  name: string;
  email: string;
  age?: number;
}

// Partial<T>: 모든 프로퍼티를 optional로
type PartialUser = Partial<User>; // { id?: number; name?: string; ... }

// Required<T>: 모든 프로퍼티를 required로
type RequiredUser = Required<User>; // age도 필수

// Readonly<T>: 모든 프로퍼티를 readonly로
type ReadonlyUser = Readonly<User>;

// Pick<T, K>: 특정 프로퍼티만 선택
type UserPreview = Pick<User, 'id' | 'name'>;

// Omit<T, K>: 특정 프로퍼티 제외
type UserWithoutEmail = Omit<User, 'email'>;

// Record<K, V>: 키-값 타입 정의
type Roles = Record<string, boolean>; // { admin: true, user: false }

// Exclude<T, U>: 유니온에서 특정 타입 제외
type NonNullable<T> = Exclude<T, null | undefined>;

// ReturnType<T>: 함수 반환 타입 추출
function getUser() { return { id: 1, name: 'Kim' }; }
type UserType = ReturnType<typeof getUser>; // { id: number; name: string }
```

</details>

---

## 제네릭 (Generics)

> TypeScript의 제네릭에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

제네릭은 **타입을 매개변수처럼 사용**하여 재사용 가능한 코드를 작성할 수 있게 합니다.

```ts
// 제네릭 없이: 타입 중복
function identityString(arg: string): string { return arg; }
function identityNumber(arg: number): number { return arg; }

// 제네릭 사용: 하나로 통합
function identity<T>(arg: T): T { return arg; }

identity<string>('hello'); // T = string
identity<number>(42);      // T = number
identity('hello');         // 타입 추론으로 T = string 자동 결정

// React에서 제네릭 활용
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

async function fetchUser(): Promise<ApiResponse<User>> {
  return fetch('/api/user').then(res => res.json());
}
```

</details>

---

## React.ComponentPropsWithoutRef vs React.ComponentPropsWithRef

> React.ComponentPropsWithoutRef와 React.ComponentPropsWithRef의 차이를 설명해주세요.

<details>
<summary>답변 보기</summary>

```ts
// ComponentPropsWithoutRef: ref 타입 제외
// forwardRef 내부에서 사용
type ButtonProps = React.ComponentPropsWithoutRef<'button'>;

// ComponentPropsWithRef: ref 타입 포함
// ref를 직접 받아야 할 때 사용
type InputProps = React.ComponentPropsWithRef<'input'>;
```

**실제 사용 예:**
```ts
// forwardRef와 함께 사용할 때는 WithoutRef
interface ButtonProps extends React.ComponentPropsWithoutRef<'button'> {
  variant?: 'primary' | 'secondary';
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant, ...props }, ref) => (
    <button ref={ref} {...props} />
  )
);
```

</details>
