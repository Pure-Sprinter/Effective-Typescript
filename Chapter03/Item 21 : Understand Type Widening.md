## Item 21: Understand Type Widening

런타임에 모든 변수는 단일 값을 가지지만 정적인 분석을 진행할 때 타입스크립트가 코드의 변수 유형을 검사한다. 상수를 사용하여 변수를 초기화하고 유형을 제공하지 않는 경우 변수가 단일 값에서 가능한 값 집합을 결정하게 된다. 이 과정을 타입스크립트에서는 확장이라고 한다.

우리가 벡터로 라이브러리를 작성한다고 가정하면 우리는 3D 벡터에 대한 타입을 적고 컴포넌트 값을 가지는 함수를 작성하면 아래와 같다.

```typescript
interface Vector3 {
  x: number;
  y: number;
  z: number;
}
function getComponent(vector: Vector3, axis: "x" | "y" | "z") {
  return vector[axis];
}
```

그러나 우리가 이를 쓰려고 하면 오류가 발생하게 된다.

```typescript
let x = "x";
let vec = { x: 10, y: 20, z: 30 };
getComponent(vec, x);
// ~ Argument of type 'string' is not assignable to
//   parameter of type '"x" | "y" | "z"'
```

이 코드가 문제를 발생하는 이유는 x의 값을 문자열로 인식을 했는데 타입을 받을 때 number 가 아니기에 에러가 발생을 하게 된다. 위 예시로 다시 돌아와서 x의 유형을 문자열로 추론할 때, TypeScript는 특수성과 유연성 사이의 균형을 맞추려고 노력한다.

```typescript
letx = "x";
x = "a";
x = "Four score and seven years ago...";

letx = "x";
x = /x|y|z/;
x = ["x", "y", "z"];
```

일반적인 규칙은 변수 유형이 선언된 후 바뀌면 안 된다는 것이다. 따라서 문자열은 string|RegExp 또는 string|string[] 또는 any보다 더 의미가 있습니다. 타입스크립트는 확장 과정을 제어할 몇가지 방법들을 제공한다. 하나는 const 이다. 만약 우리가 let 대신에 const 를 가진 변수를 선언한다면 좀 더 좁은 타입을 가진다. 실제로 const 는 기존 문제를 해결하는데 도움이 된다.

```typescript
const x = "x"; // type is "x"
let vec = { x: 10, y: 20, z: 30 };
getComponent(vec, x); // OK
```

왜냐하면 x에 재할당할 수 없기 때문에 타입스크립트는 위험성없이 좁은 타입을 추론할 수 있다. 그러나 객체나 배열에 const를 사용하는 것은 여러 이슈들을 발생시킨다. 아래의 코드는 자바스크립트에서 아래의 코드는 괜찮다.

```javascript
constv = { x: 1 };
v.x = 3;
v.x = "3";
v.y = 4;
v.name = "Pythagoras";
```

그러나 타입스크립트 상에서는 추론 과정에서 오류가 발생한다.

```typescript
constv = { x: 1 };
v.x = 3; // OK
v.x = "3";
// ~ Type '"3"' is not assignable to type 'number'
v.y = 4;
// ~ Property 'y' does not exist on type '{ x: number; }'
v.name = "Pythagoras";
// ~~~~ Property 'name' does not exist on type '{ x: number; }'
```

구체적으로 타입을 알려고 하는 기능과 유연성 사이에서 오류가 발생하게 되는데 이를 방지하고 유연하게 대처할 여러 방법들이 있다.

```typescript
const v: { x: 1 | 3 | 5 } = { x: 1 }; // Type is { x: 1 | 3 | 5; }
```

```typescript
const v1 = { x: 1, y: 2 }; // Type is { x: number; y: number; }
const v2 = {
  x: 1 as const,
  y: 2,
}; // Type is { x: 1; y: number; }
const v3 = { x: 1, y: 2 } as const; // Type is { readonly x: 1; readonly y: 2; }
```

```typescript
const a1 = [1, 2, 3]; // Type is number[]
const a2 = [1, 2, 3] as const; // Type is readonly [1, 2, 3]
```

### 기억해야할 것!!

1. TypeScript가 상수의 유형을 확장하여 추론하는 방식을 이해한다.
2. 이 행동에 영향을 미칠 수 있는 const, 타입 주석, context와 as const 방식에 익숙해야 한다.
