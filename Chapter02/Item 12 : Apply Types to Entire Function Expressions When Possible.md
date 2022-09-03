## Item 12: Apply Types to Entire Function Expressions When Possible

자바스크립트와 타입스크립트는 함수문과 함수식을 구별한다.

```typescript
function rollDice1(sides: number): number {
  /* ... */
} // Statement
const rollDice2 = function (sides: number): number {
  /* ... */
}; // Expression
const rollDice3 = (sides: number): number => {
  /* ... */
}; // Also expression
```

타입스크립트에서 함수식의 장점은 매개 변수의 타입과 반환 타입을 개별적으로 지정하는 대신 타입 선언을 전체 함수에 한 번에 적용할 수 있다는 것이다.

```typescript
type DiceRollFn = (sides: number) => number;
const rollDice: DiceRollFn = (sides) => {
  /* ... */
};
```

타입스크립트는 반복을 줄여준다. 만약 우리가 산술 함수의 역할을 하는 여러 개의 함수들을 작성한다면 아래와 같이 될 수 있다.

```typescript
function add(a: number, b: number) {
  return a + b;
}

function sub(a: number, b: number) {
  return a - b;
}

function mul(a: number, b: number) {
  return a * b;
}

function div(a: number, b: number) {
  return a / b;
}
```

위 중복 식을 하나의 함수 타입으로 선언하여 결집할 수 있다. 이를 통해 좀더 논리를 명확히 할 수 있고 모든 함수식의 반환 값이 number 임을 확인할 수 있다.

```typescript
type BinaryFn = (a: number, b: number) => number;
const add: BinaryFn = (a, b) => a + b;
const sub: BinaryFn = (a, b) => a - b;
const mul: BinaryFn = (a, b) => a * b;
const div: BinaryFn = (a, b) => a / b;
```

변수 대신에 전체 함수를 타입화시키는 것은 좀더 안전성을 제공한다. 우리가 함수를 작성할 때 이렇게 전체 함수를 하나의 타입으로 선언하여 적용하는 것이 변수와 반환 값에 대한 타입을 작성하는 것보다 좋다.

### 기억해야할 것!!

1. 전체 함수식을 타입으로 변경하여 적용하기
2. 동일한 타입을 반복적으로 작성하는 경우 함수 타입을 제외하거나 기존 형식을 찾기
3. type of fn을 사용하여 다른 함수와 일치시키기
