## Item 14: Use Type Operations and Generics to Avoid Repeating Yourself

```typescript
console.log(
  "Cylinder 1 x 1 ",
  "Surface area:",
  6.283185 * 1 * 1 + 6.283185 * 1 * 1,
  "Volume:",
  3.14159 * 1 * 1 * 1
);

console.log(
  "Cylinder 1 x 2 ",
  "Surface area:",
  6.283185 * 1 * 1 + 6.283185 * 2 * 1,
  "Volume:",
  3.14159 * 1 * 2 * 1
);

console.log(
  "Cylinder 2 x 1 ",
  "Surface area:",
  6.283185 * 2 * 1 + 6.283185 * 2 * 1,
  "Volume:",
  3.14159 * 2 * 2 * 1
);
```

위 코드를 보면 불편함을 느끼는가? 반드시 그래야 한다. 이렇게 극단적인 반복 같은 경우는 코드 상에 오류가 발생하였을 때 복사/붙여넣기하는 것만큼 수정해야 한다. 이를 줄이려면 아래와 같이 코드를 작성할 수 있다.

```typescript
constsurfaceArea = (r, h) => 2 * Math.PI * r * (r + h);
constvolume = (r, h) => Math.PI * r * r * h;
for (const [r, h] of [
  [1, 1],
  [1, 2],
  [2, 1],
]) {
  console.log(
    `Cylinder ${r} x ${h}`,
    `Surface area: ${surfaceArea(r, h)}`,
    `Volume: ${volume(r, h)}`
  );
}
```

이와 같이 아래처럼 상속을 하지 않고 타입을 반복적으로 작성하는 것도 피해야한다.

```typescript
interface Person {
  firstName: string;
  lastName: string;
}

interface PersonWithBirthDate {
  firstName: string;
  lastName: string;
  birth: Date;
}

// 아래와 같이 상속하면 반복을 피할 수 있다.
interface PersonWithBirthDate extends Person {
  birth: Date;
}

type PersonWithBirthDate = Person & { birth: Date };
```

타입 반복은 코드 상의 문제도 반복을 유발한다. 이러한 반복을 줄이는 가장 간단한 방식은 타입 이름 짓는 것을 것이다. 아래와 같이 작성하는 것보다는 새로운 타입을 만들어 사용하는 것이 낫다.

```typescript
function distance(a: { x: number; y: number }, b: { x: number; y: number }) {
  return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
}

interface Point2D {
  x: number;
  y: number;
}
function distance(a: Point2D, b: Point2D) {
  /* ... */
}
```

우리가 타입들 상태를 선언하고 같은 인터페이스를 상속받아 사용한다고 가정을 해보면 아래와 같이 State 인터페이스를 상속할 수 있다.

```typescript
interface State {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
  pageContents: string;
}

interface TopNavState {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
}
```

TopNavState를 설정해 같은 key, value를 가진 타입 속성을 정의하는 거는 반복이 발생될 수 있다. 이를 1차적으로 State를 하나의 하위 집합으로 간주하고 사용한다면 아래와 같이 인터페이스를 정의할 수 있다.

```typescript
type TopNavState = {
  userId: State["userId"];
  pageTitle: State["pageTitle"];
  recentFiles: State["recentFiles"];
};
```

이것도 길기 때문에 이를 반복문을 통하여 mapped type 으로 선언하며 코드 수를 줄일 수 있다.

```typescript
type TopNavState = {
  [k in "userId" | "pageTitle" | "recentFiles"]: State[k];
};
```

이러한 방식으로 mapped type을 선언하는 경우도 하나의 Generic 타입으로 선언할 수 있어서 또 아래와 같이 수정할 수 있다.

```typescript
type Pick<T, K> = { [k in K]: T[k] };
type TopNavState = Pick<State, "userId" | "pageTitle" | "recentFiles">;
```

이렇게 Generic 타입을 선안하는 것은 타입을 위한 함수와 동일하다. 그래서 Generic이 타입 선언을 줄이는 핵심이 되는 것은 당연하다. 하지만 Generic 타입을 쓰되 거기에 들어가는 변수를 줄이는 과정도 필요하다.

```typescript
interface Name {
  first: string;
  last: string;
}

type DancingDuo<T extends Name> = [T, T];
const couple1: DancingDuo<Name> = [
  { first: "Fred", last: "Astaire" },
  { first: "Ginger", last: "Rogers" },
]; // OK

const couple2: DancingDuo<{ first: string }> = [
  // Property 'last' is missing in type
  // '{ first: string; }' but required in type 'Name'
  { first: "Sonny" },
  { first: "Cher" },
];
```

여기서 에러가 발생을 하게 되는데 반복적인 작업으로 인해 에러가 여러 번 나타나고 수정하기도 어려워진다. 이때 우리가 앞서 사용했던 Pick 타입을 사용해서 extends 를 사용하여 작업을 아래와 같이 줄일 수 있다.

```typescript
type Pick<T, K> = {
  [k in K]: T[k];
  // ~ Type 'K' is not assignable to type 'string | number | symbol'
};

type Pick<T, K extends keyof T> = {
  [k in K]: T[k];
}; // OK

type FirstLast = Pick<Name, "first" | "last">; // OK
type FirstMiddle = Pick<Name, "first" | "middle">;
// Type '"middle"' is not assignable
// to type '"first" | "last"'
```

### 기억해야할 것!!

1. DRY(Don't Repeat Yourself) 원리는 논리에 적용하는 것만큼 타입에 적용된다.
2. 코드를 반복하기 보다 타입을 선언하라. 그리고 인터페이스 필드를 반복하는 것을 피하고 extends를 사용해라
3. 타입들 사이 매핑하기 위해 타입스크립트에 의해 제공되는 도구의 이해를 돕는다.
4. Generic 타입은 타입을 위한 함수와 동일하다. 이를 사용해 타입 반복을 줄이자.
5. Pick, Partial, ReturnType 과 같은 표준 라이브러리에 정의된 Generic 타입에 익숙해지자.
