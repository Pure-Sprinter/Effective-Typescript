## Item 1: Understand the Relationship Between TypeScript and JavaScript

만약 당신이 TypeScript를 사용한다면 "타입스크립트는 자바스크립트의 상위집합이거나 타입스크립트는 타입이 있는 자바스크립트"라는 말을 반드시 듣게 될 것이다. 그러나 이 둘 사이에 관계에 대한 이해가 아직 부족한 경우가 있다.

타입스크립트는 구문적인 측면에서 자바스크립트의 상위집합이다. 자바스크립트가 구문 에러가 없는 한 타입스크립트이기도 하다. 타입스크립트 유형 검사기가 코드와 관련된 여러 문제를 제기할 수 있다. 그러나 이는 독립적인 문제로 타입스크립트는 여전히 코드를 분석해 자바스크립트를 내보낸다.

타입스크립트 파일들은 .ts 확장자를 사용한다. 그렇다고 기것이 타입스크립트가 완전히 다른 언어라고 하는 것을 의미하지는 않는다. 왜냐하면 타입스크립트는 자바스크립트의 상위집합이기 때문에 .js 파일 안에 있는 코드는 타입스크립트이기도 하다. 따라서 main.js를 main.ts로 확장자 변경을 하더라도 바뀌지 않는다.

만약 당신이 자바스크립트를 타입스크립트 기반으로 바꾼다면 매우 도움이 될 것이다. 이는 타입스크립트로 새로 시작할 때 코드를 다시 쓸 필요가 없다는 것을 의미하며 그것에 따른 이점이 생기게 된다.

> 모든 자바스크립트 프로그램은 타입스크립트 프로그램이지만 반대는 성립하지 않는다.

```typescript
function greet(who: string) {
  console.log("Hello", who);
}
```

위 코드를 자바스크립트로 실행하게 되면 `who: string`에서 에러가 발생하게 된다. `:string`은 타입스크립트에서 특정되는 타입 어노테이션이다.

![javascript-typescript](https://user-images.githubusercontent.com/55318896/187124008-559b449a-f5ae-4191-b776-5c6631a94ad5.png)

그렇다고 해서 타입스크립트가 일반 자바스크립트의 가치를 제공하지 않는다는 것은 아닙니다.

```javascript
let city = "new york city";
console.log(city.toUppercase());
```

위 자바스크립트 코드는 실행 시 TypeError: city.toUppercase is not a function 이라는 에러가 나오게 된다. 이 프록램에는 타입 주석이 없는데 타입스크립트 형식 검사기가 문제를 발견하게 된다. 당신은 타입스크립트에게 city 유형이 문자열임을 말할 필요가 없다. 이는 초기에 값을 기반으로 추론되기 때문이다.

타입스크립트 유형 시스템의 목표 중 하나는 코드 실행 없이 runtime 예외 처리가 나올만한 코드를 발견하는 것이다. 당신이 타입스크립트를 정적인 타입 시스템으로 들었을 때 이를 의미하게 되고 형식 검사기가 항상 모든 예외를 찾을 수는 없지만 노력할 것입니다.

비록 당신의 코드가 예외처리가 안되었더라도, 의도한대로 되지 않을 수 있습니다. 타입스크립트는 이러한 이슈들을 파악하려고 하는데 아래의 예제를 보면

```javascript
const states = [
  { name: "Alabama", capital: "Montgomery" },
  { name: "Alaska", capital: "Juneau" },
  { name: "Arizona", capital: "Phoenix" },
];

for (const state of states) {
  console.log(state.capitol);
}

// undefined
// undefined
// undefined
```

이 프로그램은 분명히 유효한 자바스크립트로 에러 없이 실행하게 된다. 그러나 명확히 당신이 의도한대로 되지는 않았다. 유형 주석 추가 없이 타입 스크립트의 형식 검사기는 에러를 잡을 수 있게 된다. 예를 들어 아래 처럼 나오게 된다.

```typescript
for (const state of states) {
  console.log(state.capitol);
  // ~~~~~~~ Property 'capitol' does not exist on type
  //         '{ name: string; capital: string; }'.
  //         Did you mean 'capital'?
}
```

타입스크립트가 유형 주석을 제공하지 않더라도 에러를 잡을 수 있다면 더 꼼꼼한 작업을 진행시킬 수 있을 것입니다. 이는 유형 주석이 타입스크립트에게 당신의 의도가 뭔지 말해주고 당신의 의도와 맞지 않는 작동이 일어나는 곳을 발견하게 해줍니다.

이렇게 도움이 되는 에러는 정확히 잘못된 곳을 잡습니다! 문제는 비슷한 유형의 단어가 있다면 예를 들어, capitol/capital 둘다 있다면 타입스크립트는 어느 것이 맞는 철자인지 알 수 없어 헷갈리게 됩니다.

```typescript
const states = [
  { name: "Alabama", capitol: "Montgomery" },
  { name: "Alaska", capitol: "Juneau" },
  { name: "Arizona", capitol: "Phoenix" }, // ...
];
for (const state of states) {
  console.log(state.capital);
  // ~~~~~~~ Property 'capital' does not exist on type
  //         '{ name: string; capitol: string; }'.
  //         Did you mean 'capitol'?
}
```

위 코드를 올바르게 하려면 타입을 선언하여 명확히 당신의 의도를 나타내는 것이 중요합니다.

```typescript
interface State {
  name: string;
  capital: string;
}
const states: State[] = [
  { name: "Alabama", capitol: "Montgomery" }, // ~~~~~~~~~~~~~~~~~~~~~
  { name: "Alaska", capitol: "Juneau" }, // ~~~~~~~~~~~~~~~~~
  { name: "Arizona", capitol: "Phoenix" },
  // ~~~~~~~~~~~~~~~~~~ Object literal may only specify known
  //         properties, but 'capitol' does not exist in type
  //         'State'.  Did you mean to write 'capital'?
  // ...
];
for (const state of states) {
  console.log(state.capital);
}
```

지금은 에러가 문제를 정확히 파악하고 올바른 수정 방식을 제안합니다. 당신은 또한ㄴ 타입스크립트가 잠재적 문제를 발견하는 것에 도움을 받게 됩니다. 예를 들어, 당신이 capitol을 한번 배열에서 잘못 적었다면 문제를 알려줍니다.

```typescript
const states: State[] = [
  { name: "Alabama", capital: "Montgomery" },
  { name: "Alaska", capitol: "Juneau" },
  // ~~~~~~~~~~~~~~~~~ Did you mean to write 'capital'?
  { name: "Arizona", capital: "Phoenix" },
  // ...
];
```

### 기억해야할 것!!

1. 타입스크립트는 자바스크립트의 상위집합이다. 다른 말로하면, 모든 자바스크립트 프로그램들은 이미 타입스크립트 프로그램이다. 타입스크립트는 그 자체로 몇몇의 구문을 가지고 있어서 타입스크립트는 일반적으로 유효한 자바스크립트 프로그램이 아니다.
2. 타입스크립트는 자바스크립트 런타임에 타입 시스템을 더하고 런타임에 예외를 발생하는 코드를 발견하려 노력한다. 하지만 모든 예외를 알려준다고 기대하면 안된다.
3. 타입스크립트 유형 시스템은 크게 자바스크립트 동작을 모방하였지만 자바스크립트가 허용하는 구조가 있지만 타입스크립트는 잘못된 수의 인자로 함수를 호출하는 것처럼 막기를 선택한다. 이는 대체로 취향의 문제이다.
