## Item 23 : Create Objects All at Once

변수의 값이 바뀌지만 변수의 타입은 바뀌지 않는다. 따라서 객체를 조각별로 나눠서 생성하는 것보다 한번에 객체를 만드는 것이 좋다. 아래의 예시를 보게 되자

```typescript
const pt = {};
pt.x = 3;
// ~ Property 'x' does not exist on type '{}'
pt.y = 4;
// ~ Property 'y' does not exist on type '{}'

interface Point {
  x: number;
  y: number;
}

const pt: Point = {};
// ~~ Type '{}' is missing the following properties from type 'Point': x, y
pt.x = 3;
pt.y = 4;
```

이러한 문제를 한번에 해결하는 방법은 아래와 같다.

```typescript
const pt = { x: 3, y: 4 }; // OK

const pt = {} as Point;
pt.x = 3;
pt.y = 4; // OK
```

그러나 가장 좋은 방법은 타입 선언과 동시에 객체를 생성하는 것이다.

```typescript
const pt: Point = { x: 3, y: 4 };
```

만약 우리가 새로운 속성을 할당한다고 한다면 이때는 어떻게 대처할 것인가? 바로 스프레드 문법을 사용해 객체의 범위를 확대하는 것이다.

```typescript
const namedPoint = { ...pt, ...id };
namedPoint.name; // OK, type is string

const pt0 = {};
const pt1 = { ...pt0, x: 3 };
const pt: Point = { ...pt1, y: 4 }; // OK
```

만약 우리가 조건문을 사용하여 다른 속성을 추가한다면 아래와 같이 할 수 있다.

```typescript
declare let hasMiddle: boolean;
const firstLast = { first: "Harry", last: "Truman" };
const president = { ...firstLast, ...(hasMiddle ? { middle: "S" } : {}) };
```

### 기억해야할 것!!

1. 객체를 한번에 선언하는 것이 더 좋고 만약 type-safe 방식으로 속성을 추가한다면 객체 스프레드 문법 ({...a, ...b})을 사용하자.
2. 객체에 조건적으로 속성을 추가하는 방식을 알자.
