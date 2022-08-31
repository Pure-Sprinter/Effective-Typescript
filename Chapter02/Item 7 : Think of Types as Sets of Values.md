## Item 7: Think of Types as Sets of Values

런타임 시에 매 변수가 자바스크립트의 값들 중 하나의 값을 가지도록 노력을 하는데 여기 해당되는 다양한 값들이 존재한다.

- 42/null/undefined/'Canada'/{animal: 'Whale', weight_lbs: 40_000}
- /regex/new HTMLButtonElement/(x, y) => x + y

그러나 코드가 실행되기 전에 TypeScript가 오류를 검사할 때는 type만 있다. 이 값은 가능한 값의 집합으로 간주하는 것이 가장 좋다. 예를 들어, 숫자 타입은 모든 숫자 값의 집합이라고 생각할 수 있다. 42와 -37.25가 포함되지만 '캐나다'는 포함되지 않다. strictNullChecks에 따라 null 및 정의되지 않은 항목이 집합에 속하거나 속하지 않을 수 있다.

가장 작은 집합은 값을 포함하지 않는 빈 집합으로 이 것은 TypeScript의 never 유형에 해당한다. 도메인이 비어 있기 때문에 never 유형의 변수에 할당할 수 있는 값은 없다.

```typescript
const x: never = 12;
// ~ Type '12' is not assignable to type 'never'
```

다음으로 가장 작은 집합은 하나의 값만 포함하는 집합이다. 이러한 것들은 타입스크립트의 literal types 와 상응하는 unit 타입으로 말할 수 있다.

```typescript
type A = "A";
type B = "B";
type Twelve = 12;
```

만약 2개 혹은 3개의 값을 가진 type을 형성한다면 unit 타입들을 결합할 수 있다.

```typescript
typeAB = "A" | "B";
typeAB12 = "A" | "B" | 12;
```

"assignable"이라는 단어는 타입스크립트 오류 상에서 많이 나타난다. 값의 집합 상에서 이 단어는 "member of"(값과 타입 사이의 관계)나 "subset of"(두 개의 타입 사이의 관계)를 의미하는 것응로 볼 수 있다.

```typescript
const a: AB = "A"; // OK, value 'A' is a member of the set {'A', 'B'}
const c: AB = "C";
// ~ Type '"C"' is not assignable to type 'AB'
```

타입 C 는 unit 타입이다. C 라는 하나의 값으로 구성된 도메인으로 AB 의 하위 집합이 아니기에 오류가 나타난다. 거의 모든 타입 checker는 하나의 타입이 다른 것의 하위 집합인지 테스트를 진행을 한다.

```typescript
// OK, {"A", "B"} is a subset of {"A", "B"}:
const ab: AB = Math.random() < 0.5 ? "A" : "B";
const ab12: AB12 = ab; // OK, {"A", "B"} is a subset of {"A", "B", 12}

declare let twelve: AB12;
const back: AB = twelve;
// ~~~~ Type 'AB12' is not assignable to type 'AB'
//        Type '12' is not assignable to type 'AB'
```

이러한 타입의 집합은 유한하기 때문에 추론하기 쉽다. 그러나 우리가 실용적으로 사용하는 대부분의 타입은 무한한 영역을 가지고 있어서 추런하기란 매우 어렵다. 구조적으로 생각해보면 아래처럼 표현을 할 수 있다.

```typescript
type Int = 1 | 2 | 3 | 4 | 5; // | ...

interface Identified {
  id: string;
}
```

타입들을 값의 집합으로 생각을 하면 그것의 작동에 대한 추론을 도와줄 수 있다.

```typescript
interface Person {
  name: string;
}

interface Lifespan {
  birth: Date;
  death?: Date;
}

type PersonSpan = Person & Lifespan;
```

& 연산자는 두개의 타입의 교집합을 계산한다. 타입 연산자는 인터페이스 내 값들의 속성이 아니라 값들에 집합에 적용을 하게 되기에 추가적인 속성을 가진값들도 타입에 속하는 것을 기억해야 한다. 그래서 아래와 같은 교집합 타입을 가지게 된다.

```typescript
const ps: PersonSpan = {
  name: "Alan Turing",
  birth: new Date("1912/06/23"),
  death: new Date("1954/06/07"),
}; // OK
```

물론, 값은 3가지 속성 이상을 가질 수 있는 타입에 속하게 된다. 일반적인 규칙은 교집합 타입의 값들은 각 구성 요소의 합집합을 포함한다는 것이다. 교차 성질에 대한 직관은 정확하지만, 두 개의 교차 면이 교차하는 것이 아니라 결합하는 것에 대해 다음과 같다.

```typescript
type K = keyof (Person | Lifespan); // Type is never
```

타입스크립트가 union 유형의 값에 속한다고 보장할 수 있는 키가 없습니다. 따라서 union의 키는 빈 집합(절대)이어야 합니다. 또는, 더 공식적으로:

```typescript
keyof (A&B) = (keyof A) | (keyof B)
keyof (A|B) = (keyof A) & (keyof B)
```

아니면 PersonSpan 타입은 확장된 요소로 흔하게 쓸 수 있다.

```typescript
interface Person {
  name: string;
}

interface PersonSpan extends Person {
  birth: Date;
  death?: Date;
}
```

당신은 "subtype"이라는 단어를 들어봤을지도 모른다. 이것은 하나의 집합 영역이 다른 요소의 하위 집합이라는 말로 아래와 같이 3개의 인터페이스로 볼 수 있다.

```typescript
interface Vector1D {
  x: number;
}

interface Vector2D extends Vector1D {
  y: number;
}

interface Vector3D extends Vector2D {
  z: number;
}
```

이를 벤다이어그램으로 표현하면 이런 식으로 표현할 수 있다.

![Venn Diagram](https://user-images.githubusercontent.com/55318896/187681971-e8314615-a933-482c-90af-426745ac9117.png)

```typescript
function getKey<K extends string>(val: any, key: K) {
  // ...
}
```

여기서 string을 extends를 하는 것은 어떤 의미일까? 만약에 객체 상속에 대한 개념을 생각했다면 해석하기가 어렵다. 당신은 object wrapper type String의 하위 클래스를 정의할 수 있지만 권장하지는 않는다.

집합에 대한 개념을 생각해보면 투명하다. 도메인이 문자열의 하위 집합인 모든 유형이면 되고 여기에는 문자열 리터럴 유형, 문자열 리터럴 유형 및 문자열 자체의 결합이 포함된다.

```typescript
getKey({}, "x"); // OK, 'x' extends string
getKey({}, Math.random() < 0.5 ? "a" : "b"); // OK, 'a'|'b' extends string
getKey({}, document.title); // OK, string extends string
getKey({}, 12);
// ~~ Type '12' is not assignable to parameter of type 'string'
```

| TypeScript term       | Set term               |
| --------------------- | ---------------------- |
| never                 | ∅ (empty set)          |
| Literal type          | Single element set     |
| Value assignable to T | Value ∈ T (member of)  |
| T1 assignable to T2   | T1 ⊆ T2 (subset of)    |
| T1 extends T2         | T1 ⊆ T2 (subset of)    |
| T1 or(이걸로 대체) T2 | T1 ∪ T2 (union)        |
| T1 & T2               | T1 ∩ T2 (intersection) |
| unknown               | Universal set          |

### 기억해야할 것!!

1. 타입을 값 집합(유형의 도메인)으로 간주하고 이러한 집합은 유한(예: 부울 형식 또는 리터럴 형식) 또는 무한(예: 숫자 또는 문자열)일 수 있다.
2. TypeScript 타입은 엄격한 계층 구조가 아닌 교차 집합(벤 다이어그램)을 형성한다. 두 유형은 다른 유형의 하위 유형이 아닌 경우에도 중복될 수 있다.
3. 개체가 타입 선언에 언급되지 않은 추가 속성이 있더라도 여전히 타입에 속할 수 있다.
4. 타입 연산자는 집합의 도메인에 적용되어 A와 B의 교차점은 A의 도메인과 B의 도메인의 교차점이다. 개체 유형의 경우, 이것은 A와 B의 값이 A와 B의 속성을 모두 가지고 있음을 의미한다.
5. "extends", "assignable to" 및 "subtype of"을 "subset of"의 동의어로 생각하기
