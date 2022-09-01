## Item 9: Prefer Type Declarations to Type Assertions

타입스크립트는 값을 변수에 할당하고 타입을 주는 2가지 방법이 있다.

```typescript
interface Person {
  name: string;
}

const alice: Person = { name: "Alice" }; // Type is Person
const bob = { name: "Bob" } as Person; // Type is Person
```

비슷해보이지만 어느 정도 다르다. 첫번째 요소는 변수에 타입선언을 함으로써 이 값이 특정 타입을 가지는 것을 보장한ㄴ다. 후자는 타입을 주입시키는 방식으로 이 타입으로 추론될 수 있다는 것이다. 일반적으로, 우리는 타입 선언을 타입 주입보다 더 선호해야한다.

```typescript
const alice: Person = {};
// Property 'name' is missing in type '{}'
// but required in type 'Person'
const bob = {} as Person; // No error
```

타입 선언은 인터페이스가 가진 값들을 보장한다. 타입 주입은 이러한 오류를 말하지 않고 지나가게 된다. 같은 일은 추가적인 속성을 명시할 때도 발생한다.

```typescript
const alice: Person = {
  name: "Alice",
  occupation: "TypeScript developer",
  // ~~~~~~~~~ Object literal may only specify known properties
  // and 'occupation' does not exist in type 'Person'
};
const bob = {
  name: "Bob",
  occupation: "JavaScript developer",
} as Person; // No error
```

우리가 함수형 프로그래밍 상에서 타입 선언을 하며 타입을 바꾸려면 어떻게 해야할까?

```typescript
const people = ["alice", "bob", "jan"].map((name) => ({ name }));
// { name: string; }[]... but we want Person[]
```

위 코드를 아래의 2가지 방식으로 바꿀 수 있지만 가장 좋은 것은 후자 요소를 넣는 것이다.

```typescript
const people = ["alice", "bob", "jan"].map((name) => {
  const person: Person = { name };
  return person;
}); // Type is Person[]

const people = ["alice", "bob", "jan"].map((name): Person => ({ name }));
// Type is Person[]
```

타업 Assertion(타입 주입이라고 하자)는 어디서 사용할까?? 우리가 페이지 작업을 하다보면 DOM API라는 것을 알지만 컴퓨터 상에서 추론을 못하는 경우가 존재한다. 이 때 타입을 주입할 수 있다.

```typescript
document.querySelector("#myButton").addEventListener("click", (e) => {
  e.currentTarget; // Type is EventTarget
  const button = e.currentTarget as HTMLButtonElement;
  button; // Type is HTMLButtonElement
});
```

### 기억해야할 것!!

1. 타입 선언(: Type)을 타입 주입(as Type)보다 선호하자.
2. 화살표 함수의 반환 타입을 명시하는 방법을 알자.
3. TypeScript에 없는 유형에 대해 알고 있는 경우 타입 주입과 Null이 아닌 주입을 사용하자.
