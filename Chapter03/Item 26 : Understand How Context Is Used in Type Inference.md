## Item 26: Understand How Context Is Used in Type Inference

타입스크립트는 단순히 값을 기반으로 타입을 추론하는 것이 아니다. 값이 발생하는 상황을 고려해 타이블 추론한다.

```typescript
function setLanguage(language: string) {
  /* ... */
}

// Inline form
setLanguage("JavaScript"); // OK

// Reference form
let language = "JavaScript";
setLanguage(language); // OK
```

```typescript
type Language = "JavaScript" | "TypeScript" | "Python";
function setLanguage(language: Language) {
  /* ... */
}

// Inline form
setLanguage("JavaScript"); // OK

// Reference form
let language = "JavaScript";
setLanguage(language);
// ~~~~~~~~ Argument of type 'string' is not assignable
//          to parameter of type 'Language'
```

인라인 폼에서 TypeScript는 함수 선언을 통해 매개 변수가 Language 유형이어야 한다는 것을 알 수 있다. 문자열 리터럴 'JavaScript'는 이 유형에 할당할 수 있으므로, 이것은 괜찮다. 그러나 변수를 제외할 때 TypeScript는 할당 시 해당 유형을 추론해야 하기에 언어에 할당할 수 없는 문자열을 추론하게 되어 오류가 발생하게 된다.

따라서 이를 해결하기 위해서는 타입의 범위를 좁히는 2가지 방식을 사용해야 한다. 첫번째는 처음부터 타입을 선언하는 것이고 두번째는 const 를 하는 것이다.

```typescript
let language: Language = "JavaScript";
setLanguage(language); // OK

const language = "JavaScript";
setLanguage(language); // OK
```

### Tuple Types

문자열 리터럴 타입에 더해 문제는 튜플 타입에도 발생한다.

```typescript
// Parameter is a (latitude, longitude) pair.
function panTo(where: [number, number]) {
  /* ... */
}
panTo([10, 20]); // OK
const loc = [10, 20];
panTo(loc);
// ~~~ Argument of type 'number[]' is not assignable to
// parameter of type '[number, number]'
```

원래는 [number, number] 형식임을 추론하여 허용을 해야하는데 추론하지 못하기 때문에 아래 방식처럼 수정해줘야 한다.

```typescript
const loc: [number, number] = [10, 20];
panTo(loc); // OK

const loc = [10, 20] as const;
panTo(loc);
// ~~~ Type 'readonly [10, 20]' is 'readonly'
//     and cannot be assigned to the mutable type '[number, number]'
```

### Objects

값을 문맥과 분리하는 문제점은 큰 객체를 만들 때 발생을 하게 된다.

```typescript
type Language = "JavaScript" | "TypeScript" | "Python";
interface GovernedLanguage {
  language: Language;
  organization: string;
}
function complain(language: GovernedLanguage) {
  /* ... */
}
complain({ language: "TypeScript", organization: "Microsoft" }); // OK

const ts = {
  language: "TypeScript",
  organization: "Microsoft",
};
complain(ts);

// ~~ Argument of type '{ language: string; organization: string; }'
// is not assignable to parameter of type 'GovernedLanguage'
// Types of property 'language' are incompatible
// Type 'string' is not assignable to type 'Language'
```

여기서 발생하는 ts 객체도 앞서 해결한 방식처럼 const ts: GovernedLanguage 방식으로 해결이 가능하다.

### Callbacks

다른 함수에 콜백을 넘길 때, 타입스크립트는 콜백의 매개변수 타입을 문맥을 이용하여 추론한다.

```typescript
function callWithRandomNumbers(fn: (n1: number, n2: number) => void) {
  fn(Math.random(), Math.random());
}

callWithRandomNumbers((a, b) => {
  a; // Type is number
  b; // Type is number console.log(a + b);
});
```

### 기억해야할 것!!

1. 타입 추론에서 문맥이 어떻게 사용되는지 인지하기
2. 변수를 인수 분해하면 유형 오류가 발생하는 경우 유형 선언을 추가하는 것을 고려하기
3. 변수가 실제로 상수인 경우 const를 사용합니다.
