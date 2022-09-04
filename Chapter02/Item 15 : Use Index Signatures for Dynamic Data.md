## Item 15: Use Index Signatures for Dynamic Data

우리가 자바스크립트 상에서 객체를 표현할 때 아래와 같이 코드를 작성하게 된다.

```javascript
const rocket = { name: "Falcon 9", variant: "Block 5", thrust: "7,607 kN" };
```

코드를 보면 값 타입이 문자열 임을 알 수 있다. 타입스크립트는 유연한 타입 매핑을 지원해준다. 그래서 위 코드를 타입스크립트 상에 작성을 하면 아래와 같이 선언할 수 있다.

```typescript
type Rocket = { [property: string]: string };
const rocket: Rocket = {
  name: "Falcon 9",
  variant: "v1.0",
  thrust: "4,940 kN",
}; // OK
```

여기서 `[property: string]: string`이 있는데 이 요소는 3가지 요소를 특정하게 된다.

1. A name for the keys : 순수하게 글자로 나타내는 키 이름으로 Type Checker 가 사용되지 않는다.
2. A type for the key : string, number, symbol과 같은 조합이 필요하지만 일반적으로 string이다.
3. A type for the values : 어느 값이나 될 수 있다.

이 경우 유형 검사를 수행하지만 몇 가지 단점이 있다.

- 잘못된 키를 포함하여 모든 키를 허용한다.
- 특정 키가 있어야 할 필요는 없습니다. {} 또한 유효하다.
- 다른 키에 대해 고유한 유형을 가질 수 없습니다.
- TypeScript의 언어 서비스는 name:을(를) 입력할 때 키가 아무거나 될 수 있으므로 자동 완성은 없다.

그래서 보통은 interface로 선언하여 사용하는 것이 좋다.

```typescript
interface Rocket {
  name: string;
  variant: string;
  thrust_kN: number;
}

const falconHeavy: Rocket = {
  name: "Falcon Heavy",
  variant: "v1",
  thrust_kN: 15_200,
};
```

우리가 객체에 사용하는 여러 키/값 구조를 적용해서 타입을 선언하며 코드를 줄일 수 있다. 아래의 코드를 한번 보자.

```typescript
interface Row1 {
  [column: string]: number;
} // Too broad

interface Row2 {
  a: number;
  b?: number;
  c?: number;
  d?: number;
} // Better

type Row3 =
  | { a: number }
  | { a: number; b: number }
  | { a: number; b: number; c: number }
  | { a: number; b: number; c: number; d: number };
```

이를 아래의 요소와 같은 Generic Type을 사용하면 코드의 반복을 줄일 수 있다.

```typescript
type Vec3D = { [k in "x" | "y" | "z"]: number };
// Type Vec3D = {
// x: number;
// y: number;
//   z: number;
// }

type ABC = { [k in "a" | "b" | "c"]: k extends "b" ? string : number }; // Type ABC = {
// a: number;
// b: string;
//   c: number;
// }
```

### 기억해야할 것!!

1. CSV 파일에서 개체 속성을 로드하는 경우와 같이 런타임까지 개체 속성을 알 수 없는 경우 인덱스 서명을 사용한다.
2. 안전한 액세스를 위해 인덱스 서명 값 유형에 정의되지 않은 항목을 추가하는 것을 고려해보기
3. 인터페이스, 레코드 또는 매핑된 형식과 같이 가능한 경우 signature index보다 더 정확한 형식을 선호한다.
