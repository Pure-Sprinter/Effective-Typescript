## Item 8: Know How to Tell Whether a Symbol Is in the Type Space or Value Space

타입스크립트 symbol은 둘 중 하나의 공간에 존재한다.

1. Type space
2. Value space

이 개념은 혼란을 가져온다. 왜냐하면 같은 이름을 가진 요소가 서로 다른 공간을 의존하고 있을 수 있기 때문이다.

```typescript
interface Cylinder {
  radius: number;
  height: number;
}

const Cylinder = (radius: number, height: number) => ({ radius, height });
```

Cylinder 인터페이스는 type space 내 symbol을 보여주고 const Cylinder는 value space 에 존재하는 같은 이름이다. 서로 관계는 없지만 우리가 Cylinder 타입 생각할 때 어떤 타입의 값을 사용해야 하는지 몰라 오류가 발생하게 된다.

```typescript
function calculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    shape.radius;
    // ~~~~~~ Property 'radius' does not exist on type '{}'
  }
}
```

타입스크립트 사에서 문구를 type space와 value space 사이로 대체하게 된다. 이 때 :(type declaration) 혹은 as(assertion) 를 통하여 타입 공간을 확보하고 = 를 통하여는 value space 를 차지하게 된다.

```typescript
interface Person {
  first: string;
  last: string;
}
const p: Person = { first: "Jane", last: "Jacobs" };
// - --------------------------------- Values // ------ Type

function email(p: Person, subject: string, body: string): Response {
  //     ----- -          -------          ----  Values
  //              ------           ------        ------   -------- Types
  // ...
}
```

class와 enum 구조는 type과 value 둘다 보여준다.

```typescript
class Cylinder {
  radius = 1;
  height = 1;
}

function calculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    shape; // OK, type is Cylinder
    shape.radius; // OK, type is number }
  }
}
```

type, value context 안에 수많은 연산자와 키워드가 존재한다.

```typescript
type T1 = typeof p; // Type is Person
type T2 = typeof email;
// Type is (p: Person, subject: string, body: string) => Response

const v1 = typeof p; // Value is "object"
const v2 = typeof email; // Value is "function"
```

위 코드에 type context 상에서 typeof 는 값을 보고 타입스크립트의 타입으로 변환해준다.

value context 상에서 typeof 는 자바스크립트 런타임 typeof 연산자를 반호나한다. symbol 의 runtime 타입을 포함하는 문자열을 반환한다. 이는 타입스크립트의 타입과 같지 않다. 자바스크립트 런타임 타입 시스템은 타입스크립트 정적 타입시스템보다 더 간단하다. 무한한 다양성을 가진 타입스크립트 타입과 반대로 자바스크립트에는 오직 6개의 런타임 타입들이 존재한다: “string,” “number,” “boolean,” “undefined,” “object,”, “function.” 이 있다.

typeof 는 항상 값에 대한 작동을 ㅇ한다. 이를 type 에 적용할 수 없다. class 키워드는 값과 타입 둘다 가진다. 그래서 typeof 를 사용하면 어떻게 될까? 이는 문맥 상황에 달려있다.

```typescript
const v = typeof Cylinder; // Value is "function"
type T = typeof Cylinder; // Type is typeof Cylinder
```

자바스크립트에서 class가 실행되는 모습으로 보아 value는 "function"이다. 여기서 중요한 거는 constructor 함수로 new 로 선언되는 것이다.

```typescript
declare let fn: T;
const c = new fn(); // Type is Cylinder
```

이러한 2가지 공간에는 다른 의미들을 가진 수많은 construct가 존재한다.

- value space 에 있는 `this`는 자바스크립트의 `this` 키워드이다. 타입으로서 `this`는 타입스크립트의 `this` 타입이다. (다형성 this)
- value space 에서 &와 | 는 둘다 AND와 OR을 의미하며 type space에서 이 요소들은 타입 교집합 및 합집합이 된다.
- const 는 새로운 변수를 소개해주지만 as const 는 표현상에서 추론된 타입으로 바꾼다.
- extends 는 하위 클래스와 하위 집합을 정의할 수 있다. 또한 generic type의 제약 요소가 될 수 있다.
- in 은 반복문의 일부 혹은 mapped type 이 될 수 있다.

### 기억해야할 것!!

1. 타입스크립트 표현식을 읽으면서 type space 혹은 value space에 있는지 아닌지 알아야 한다.
2. 모든 값은 타입을 가지고 있지만 타입은 값을 가지고 있지 않다. type 이나 interface 와 같은 Constructs는 type space 내에 있다.
3. "foo"는 string literal 이거나 string literal type 일지 모른다. 이러한 요소를 구별하고 각각이 어떻게 동작하는지 유심히 봐야 한다.
4. typeof, this와 많은 연산자와 키워드는 type space나 value space 내부에 다른 의미들을 가지고 있다.
5. 몇몇 class, enum 과 같은 constructs들은 type이나 value를 가지고 있다.
