# Typescript

- [모르는 타입에 대해서 any가 아닌 unknown을 사용합니다.](#모르는-타입에-대해서-any가-아닌-unknown을-사용합니다)
- [union 타입, unknown 타입에 대해 타입 가드를 사용합니다.](#union-타입-unknown-타입에-대해-타입-가드를-사용합니다)
- [interface를 선언할 때 I prefix를 사용하지 않고 Pascal case로 타입을 선언합니다.](#interface를-선언할-때-i-prefix를-사용하지-않고-pascal-case로-타입을-선언합니다)
- [DRY (Don't repeat yourself)](#dry-don't-repeat-yourself)
  - [같은 타입을 여러번 선언하지 마세요.](#같은-타입을-여러번-선언하지-마세요)
  - [추론된 타입을 다시 선언하지 마세요.](#추론된-타입을-다시-선언하지-마세요)
  - [유틸리티 타입을 사용하세요.](#유틸리티-타입을-사용하세요)
- [열거형은 `enum`은 사용하지 않고 `as const`를 사용하여 표현합니다.](#열거형은-enum은-사용하지-않고-as-const를-사용하여-표현합니다)
- [컴포넌트의 Props의 타입 선언은 같은 파일 내에 Props라는 네이밍으로 선언합니다.](#컴포넌트의-props의-타입-선언은-같은-파일-내에-props라는-네이밍으로-선언합니다)

## 모르는 타입에 대해서 any가 아닌 unknown을 사용합니다.

```ts
//DON'T
function handleSpartanWeapon(weapon: any) {
  weapon.swing();
}

//DO
function handleSpartanWeapon(weapon: unknown) {
  if (weapon instanceof Spear) {
    weapon.swing();
  } else {
    console.log("르탄이의 무기 유형을 알 수 없습니다.");
  }
}
```

> why?

- any는 타입을 완전히 무시합니다. 그렇기 때문에 런타임에 오류가 발생할 확률이 높습니다.
- any는 그 영향의 범위가 굉장히 넓기 때문에 타입시스템이 오류를 잡아내기 어렵게 만듭니다.
- unknown 타입은 코드 내에서 데이터의 타입을 확인하도록 강제하여, 프로그램의 안정성을 높입니다.

## union 타입, unknown 타입에 대해 타입 가드를 사용합니다.

```ts
type StringOrNumber = string | number;

function processValue(value: StringOrNumber) {
  if (typeof value === "string") {
    console.log(`String value: ${value.toUpperCase()}`);
  } else {
    console.log(`Number value: ${value.toFixed(2)}`);
  }
}

function isString(value: unknown): value is string {
  return typeof value === "string";
}

function safelyProcessUnknown(value: unknown) {
  if (isString(value)) {
    console.log(`String value: ${value.toUpperCase()}`);
  } else {
    console.log("Not a string");
  }
}
```

> caution

함수의 반환 타입으로 union 타입 사용을 사용할 때는 다음과 같은 대안을 항상 고민하세요. union 타입 반환은 호출부의 반복적인 타입 가드를 유발합니다.

- 함수 내부에서 예외처리
- 단일 타입을 반환하는 메서드로 분리

> why?

- 우리 팀은 `unknown` 타입을 사용하기로 결정했습니다. `unknown` 타입의 변수를 사용할 때는 타입 가드를 적용해야, 타입이 안전하게 검증되어 컴파일 오류를 피할 수 있습니다
- union 타입을 구체화 하지 않았을 때 타입에 공통으로 존재하지 않는 프로퍼티에 접근할 시 오류가 납니다.

## interface를 선언할 때 I prefix를 사용하지 않고 Pascal case로 타입을 선언합니다.

```ts
//DON'T
interface ISpartanWarrior {
  name: string;
  weapon: string;
}

//DO
interface SpartanWarrior {
  name: string;
  weapon: string;
}
```

> why?

- 타입스크립트에서는 `type`과 `interface`를 교차하여 사용할 수 있습니다. 여기서 i 접두어는 오히려 불필요한 구분을 초래합니다.
- I prefix는 명확한 타입명을 설계하는 것을 방해합니다. 그 의미와 범위를 충분히 설명하지 않아도 I 만 붙이면 되기 때문에 큰 고민없이 사용할 수 있기 때문입니다.
- 저희 팀은 I prefix가 사용자에게 유효한 정보를 제공하지 않는다는 것에 동의했습니다.

## DRY (Don't repeat yourself)

### 같은 타입을 여러번 선언하지 마세요.

같은 타입은 한 번만 정의하고 필요한 곳에서 재사용하세요.

```ts
interface UserData {
  id: number;
  name: string;
  email: string;
}

function getUserData(): UserData {
  return { id: 1, name: "르탄이", email: "teamsparta@example.com" };
}

function setUserDetails(user: UserData) {
  console.log(`User ${user.name} details updated.`);
}
```

### 추론된 타입을 다시 선언하지 마세요.

가능한 경우 타입스크립트의 타입 추론 기능을 활용하여 코드를 간결하게 유지하세요.

```ts
//Don't
const userName: string = "르탄이";

function calculateArea(radius: number): number {
  return Math.PI * radius * radius;
}

//Do
const userName = "르탄이";

function calculateArea(radius: number) {
  return Math.PI * radius * radius;
}
```

### 유틸리티 타입을 사용하세요.

유틸리티 타입을 사용하여 기존의 타입들을 조합하거나 변형함하여 새로운 타입을 생성하세요.

```ts
interface SpartanWarrior {
  name: string;
  age: number;
  rank: string;
  retired: boolean;
}


const updateWarrior = (warrior: Partial<SpartanWarrior>) => {
  ...
};

const displayWarrior = (warrior: Readonly<SpartanWarrior>) => {
  console.log(`${warrior.name}, ${warrior.rank}`);
};

type WarriorPreview = Pick<SpartanWarrior, 'name' | 'rank'>;

type ActiveWarrior = Omit<SpartanWarrior, 'retired'>;

const warriorAges: Record<string, number> = {
  르탄이 : 26
};
```

> why?

- 타입 중복을 줄이면 수정할 부분이 적어집니다.
- 중복 코드가 적을 수록 코드베이스를 이해하고 관리하기 더 쉬워집니다.

## 열거형은 `enum`은 사용하지 않고 `as const`를 사용하여 표현합니다.

```ts
//Don't
enum rtannyStatus {
  PLAYING = "Playing",
  LEARNING = "Learning",
  CODING = "Coding",
  RESTING = "Resting",
}

//Do
const rtannyStatus = {
  PLAYING: "Playing",
  LEARNING: "Learning",
  CODING: "Coding",
  RESTING: "Resting",
} as const;
```

> why?

- enum은 tree-shaking되지 않아 패키지 용량을 증가시킵니다.
- enum의 양방향 매핑 특징이 장점보다 단점이 많다는 것에 합의했습니다.

## 컴포넌트의 Props의 타입 선언은 같은 파일 내에 Props라는 네이밍으로 선언합니다.

```tsx
interface Props {
  name: string;
  age: number;
  isAdmin: boolean;
}

const UserCard = ({ name, age, isAdmin }: Props) => {
  return (
    <div className="user-card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      {isAdmin && <p>(Admin)</p>}
    </div>
  );
};
```

> why?

- 한 파일내에 하나의 컴포넌트를 선언하기 때문에 Props라는 네이밍은 해당 컴포넌트의 prop에 대한 타입이라는 정보를 쉽게 전달할 수 있습니다.
