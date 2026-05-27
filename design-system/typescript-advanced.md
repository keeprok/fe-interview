# 🔷 헤드리스 디자인 시스템 - TypeScript 심화

> 우선순위: ★★☆ (차별화 포인트)

## 목차
- [Polymorphic 컴포넌트](#polymorphic-컴포넌트)
- [Omit을 사용한 이유](#omit을-사용한-이유)
- [forwardRef와 제네릭을 함께 쓸 때 타입 단언이 필요한 이유](#forwardref와-제네릭을-함께-쓸-때-타입-단언이-필요한-이유)
- [유틸리티 타입 활용](#유틸리티-타입-활용)

---

## Polymorphic 컴포넌트

> ★★☆ Polymorphic 컴포넌트가 무엇인지, 왜 설계했는지 설명해주세요.

<details>
<summary>답변 보기</summary>

**Polymorphic 컴포넌트**는 `as` prop을 통해 **렌더링할 HTML 요소나 컴포넌트를 런타임에 결정**할 수 있는 패턴입니다.

**왜 필요한가?**
- Button 컴포넌트를 `<button>`, `<a>`, `<Link>` 등으로 유연하게 렌더링하고 싶을 때

```tsx
// 사용 예시
<Button as="a" href="/about">링크처럼 보이는 버튼</Button>
<Button as={Link} to="/about">React Router Link</Button>
<Button>일반 버튼</Button>
```

**TypeScript 구현:**
```tsx
type AsProp<C extends React.ElementType> = {
  as?: C;
};

type PropsToOmit<C extends React.ElementType, P> = keyof (AsProp<C> & P);

type PolymorphicComponentProps<
  C extends React.ElementType,
  Props = {}
> = React.PropsWithChildren<Props & AsProp<C>> &
  Omit<React.ComponentPropsWithoutRef<C>, PropsToOmit<C, Props>>;

// Button 적용
type ButtonOwnProps = {
  variant?: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
};

type ButtonProps<C extends React.ElementType = 'button'> =
  PolymorphicComponentProps<C, ButtonOwnProps>;

function Button<C extends React.ElementType = 'button'>({
  as,
  variant = 'primary',
  children,
  ...rest
}: ButtonProps<C>) {
  const Component = as || 'button';
  return <Component {...rest}>{children}</Component>;
}
```

**`React.ElementType`이 뭔가요?**
- HTML 태그 문자열(`'button'`, `'a'`, `'div'`)이나 React 컴포넌트를 모두 포함하는 타입

</details>

---

## Omit을 사용한 이유

> Polymorphic 컴포넌트에서 Omit을 쓴 이유가 무엇인가요?

<details>
<summary>답변 보기</summary>

**충돌 방지**가 핵심 이유입니다.

```tsx
// 문제 상황: Button의 variant prop과 HTML 속성이 충돌할 수 있음
type ButtonProps = ButtonOwnProps & React.ComponentPropsWithoutRef<'button'>;
// 만약 HTML button에 'variant'라는 속성이 있다면 타입이 충돌

// Omit으로 자체 props 키를 HTML 속성에서 제거
type ButtonProps<C extends React.ElementType> =
  ButtonOwnProps &
  Omit<
    React.ComponentPropsWithoutRef<C>,
    keyof ButtonOwnProps  // 자체 props와 겹치는 HTML 속성 제거
  >;
```

**실용적 예시:**
```tsx
// Button에 'size'가 있는데, HTML 속성에도 'size'가 있을 수 있음
// Omit으로 HTML의 'size'를 제거하고 Button의 'size'(sm/md/lg)만 사용
```

</details>

---

## forwardRef와 제네릭을 함께 쓸 때 타입 단언이 필요한 이유

> forwardRef와 제네릭을 함께 쓸 때 왜 타입 단언(as)이 필요한가요?

<details>
<summary>답변 보기</summary>

TypeScript에서 `React.forwardRef`는 **제네릭 함수를 직접 지원하지 않습니다**.

```tsx
// 문제: forwardRef는 제네릭 컴포넌트를 반환하지 못함
const Button = React.forwardRef(<C extends React.ElementType = 'button'>(
  props: ButtonProps<C>,
  ref: React.ForwardedRef<...>
) => { ... }); // 타입 에러

// 해결: 타입 단언(as)으로 강제 지정
const Button = React.forwardRef((...) => { ... }) as <C extends React.ElementType>(
  props: ButtonProps<C> & { ref?: React.Ref<...> }
) => React.ReactElement;
```

**이유:** `forwardRef`의 반환 타입은 제네릭을 유지하는 타입 시스템이 아직 완벽하지 않기 때문. 이는 TypeScript의 알려진 한계입니다.

</details>

---

## 유틸리티 타입 활용

> Omit, Pick, Partial, Required의 차이를 설명해주세요.

<details>
<summary>답변 보기</summary>

```ts
interface ButtonProps {
  variant: 'primary' | 'secondary';
  size: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  children: React.ReactNode;
}

// Partial<T>: 모든 프로퍼티를 optional로
type PartialButtonProps = Partial<ButtonProps>;
// { variant?: ...; size?: ...; disabled?: ...; children?: ... }

// Required<T>: 모든 프로퍼티를 required로
type RequiredButtonProps = Required<ButtonProps>;
// disabled도 필수

// Pick<T, K>: 특정 프로퍼티만 선택
type ButtonVariantProps = Pick<ButtonProps, 'variant' | 'size'>;

// Omit<T, K>: 특정 프로퍼티 제외
type ButtonWithoutChildren = Omit<ButtonProps, 'children'>;
```

**실제 사용 패턴:**
```tsx
// 컴포넌트 props에서 특정 props만 외부 노출
type SelectTriggerProps = Omit<
  React.ComponentPropsWithoutRef<'button'>,
  'onClick'  // 내부에서 제어하므로 외부에서 받지 않음
>;
```

</details>
