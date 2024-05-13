# Naming

- [상수네이밍](#상수-네이밍)
- [약어 표기법](#약어-표기법)
- [객체-속성 표기법](#객체-속성-표기법)
- [핸들러 함수 표기법](#핸들러-함수-표기법)
- [스타일 컴포넌트 표기법](#스타일-컴포넌트-표기법)
- [복수형 표현 컨벤션](#복수형-표현-컨벤션)
- [Boolean 변수](#boolean-변수)
- [함수 접두사](#함수-접두사)
  - [Boolean 값 반환 함수](#boolean-값-반환-함수)
  - [데이터 처리 함수](#데이터-처리-함수)
- [Props의 타입 네이밍](#props의-타입-네이밍)

## 상수 네이밍

상수는 대문자, 스테이크 케이스로 표기합니다

```ts
//DON'T
const OpenHours = 24;

//DO
const OPEN_HOURS = 24;
```

## 약어 표기법

약어는 첫 시작 글자만 대문자로 합니다.

```ts
//DON'T
const FAQ = {...}

//DO
const Faq = {...}
```

## 객체-속성 표기법

속성에서 객체 이름을 중복해서 적지 않습니다.

```ts
//DON'T
const flower = {
  flowerColor: red,
  flowerScent: rose,
};

//DO
const flower = {
  color: red,
  scent: rose,
};
```

## 핸들러 함수 표기법

handle + 동작 + 타겟 순서로 표기합니다.

```ts
//DON'T
const handleClick = () => {};
const onSubmitButtonClick = () => {};

//DO
const handleClickSubmitButton = () => {};
```

## 스타일 컴포넌트 표기법

스타일 컴포넌트는 import _ as S 구문을 사용하며, 공통 스타일은 import _ as CS로 표기합니다.

```ts
//DON'T
import { Container, Wrapper } from "./Bakery.style.ts";

//DO
import * as S from "./Bakery.style.ts";
import * as CS from "./Common.style.ts";

return (
  <S.Container>
    <S.Wrapper>
      <S.Title>
        <CS.SubTitle>제목</CS.SubTitle>
      </S.Title>
    </S.Wrapper>
  </S.Container>
);
```

## 스타일 컴포넌트의 Container, Wrapper

### 사용 규칙

- Container: 여러 개의 자식 컴포넌트를 포함할 때 사용합니다.
- Wrapper: 단일 자식 컴포넌트를 감싸는 경우 사용합니다.

### 네이밍 세부 지침

- 한 컴포넌트의 최상단에서 Container, Wrapper를 사용할 수 있습니다.
- 하위의 Container, Wrapper는 더 디테일한 네이밍을 포함하여, 컴포넌트의 역할과 내용을 더 명확하게 표현합니다.

```tsx
import * as S from "./Dashboard.style";

return (
  <S.Container>
    <S.Wrapper>
      <S.DetailedItem>
        <S.DetailItemImgWrapper>
          <S.DetailedItemImg />
        </S.DetailItemImgWrapper>
      </S.DetailedItem>
    </S.Wrapper>
  </S.Container>
);
```

## 복수형 표현 컨벤션

- 복수형은 가능한 -es 또는 -s 접미사를 사용하여 표현합니다.
- 영어 복수형의 일반 규칙에 따라, 단어가 특별한 규칙을 따르는 경우 (예: child → children, mouse → mice), 해당 규칙을 적용합니다.

```ts
// DON'T
const userList = [{ name: "Alice" }, { name: "Bob" }];

// DO
const users = [{ name: "Alice" }, { name: "Bob" }];
```

## Boolean 변수

- is: 상태나 조건을 나타내는 불리언(Boolean) 값에 사용합니다.
- has: 소유하거나 포함하는 상태를 나타내는 불리언 값에 사용합니다.

```ts
const isActive = true;
const hasChildren = false;
```

## 함수 접두사

### Boolean 값 반환 함수

- is: 상태나 조건을 확인하여 boolean 값을 반환하는 함수에 사용합니다.
- has: 객체나 배열 등이 특정 요소를 포함하고 있는지 확인하여 boolean 값을 반환하는 함수에 사용합니다. 예: hasChildren(), hasKey().

```ts
function isActive(user) {
  return user.active;
}
function hasChildren(node) {
  return node.children.length > 0;
}
```

### 데이터 처리 함수

- get: 데이터를 조회하여 반환하는 함수에 사용합니다.
- set: 주어진 값을 사용하여 객체의 상태를 변경하는 함수에 사용합니다.
- handle: 이벤트 처리나 특정 로직을 수행하는 함수에 사용합니다.

```ts
function getUser(id) {
  return database.find((user) => user.id === id);
}
function setUser(user) {
  database.save(user);
}
function handleClick(event) {
  console.log("Button clicked", event);
}
```

## Props의 타입 네이밍

컴포넌트의 props 타입은 Props로 명명합니다.

```tsx
interface Props {
  onClick: () => void;
  label: string;
  disabled?: boolean;
}

const Button: Props = ({ onClick, label, disabled = false }) => {
  return (
    <button onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
};
```
