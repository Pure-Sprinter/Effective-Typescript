## Item 13: Know the Differences Between type and interface

만약 우리가 선언한 타입을 정의하고 싶다면 타입을 사용하거나 인터페이스를 사용하는 2가지 방식이 있다. 클래스를 사용할 수 있지만 자바스크립트 런타임 과정에서 타입이라기 보다 value 로 인식된다.

```typescript
type TState = { name: string; capital: string };

interface IState {
  name: string;
  capital: string;
}
```

type, interface 둘 중 무엇을 써야할까? 이 둘의 차이점에 대한 인지를 해야한다. 하지만 우리는 둘 다 같은 타입으로 쓰는 방법을 잘 알고 있을 필요가 있다.

첫째로, 비슷한점은 다음과 같다. State 은 거의 서로 구별할 수 없다. 만약 우리가 IState 를 정의하거나 추가 속성을 가진 TState 값을 정의한다면 문자별로 동일할 것이다.

```typescript
const wyoming: TState = {
  name: "Wyoming",
  capital: "Cheyenne",
  population: 500_000,
  // ~~~~~~~~~~~~~~~~~~ Type ... is not assignable to type 'TState'
  //                    Object literal may only specify known properties, and
  // 'population' does not exist in type 'TState'
};
```

우리는 interface 와 type 둘 다 index를 사용할 수 있다.

```typescript
type TDict = { [key: string]: string };
interface IDict {
  [key: string]: string;
}
```

또한 각각 함수 타입으로 정의할 수 있다.

```typescript
type TFn = (x: number) => string;
interface IFn {
  (x: number): string;
}
const toStrT: TFn = (x) => "" + x; // OK
const toStrI: IFn = (x) => "" + x; // OK
```

타입 별칭은 직관적인 함수 타입으로 좀 더 자연스럽지만 만약 타입이 속성들을 잘 가지고 있다면 선언된 요소들은 아래와 같을 것이다.

```typescript
type TFnWithProperties = { (x: number): number; prop: string };

interface IFnWithProperties {
  (x: number): number;
  prop: string;
}
```

우리는 이러한 구문을 잘 기억해야 한다. 타입과 인터페이스는 generic 할 수 있다.

```typescript
type TPair<T> = { first: T; second: T };

interface IPair<T> {
  first: T;
  second: T;
}
```

인터페이스는 타입을 확장할 수 있고 타입도 인터페이스를 확장할 수 있다.

```typescript
interface IStateWithPop extends TState {
  population: number;
}

type TStateWithPop = IState & { population: number };
```

다시 말하지만, 이 유형들은 동일하다. 주의할 점은 인터페이스가 union 유형과 같은 복잡한 유형을 확장할 수 없다는 것입니다. 그렇게 하려면 type과 &을 사용해야 합니다.

클래스에 동일한 인터페이스와 타입으로 적용을 하려면 아래와 같다.

```typescript
class StateT implements TState {
  name: string = "";
  capital: string = "";
}

class StateI implements IState {
  name: string = "";
  capital: string = "";
}
```

이제 차이점은 무엇일까?? 우리는 이미 한번 union type을 본적있지만 union interface는 본 적이 없다.

```typescript
type AorB = "a" | "b";
```

union 타입을 확장하는 것은 매우 유용하다. 만약 우리가 Input, Output에 구별된 타입을 가지고 이름과 변수를 매칭하면 아래와 같이 적용할 수 있다.

```typescript
type Input = {
  /* ... */
};
type Output = {
  /* ... */
};

interface VariableMap {
  [name: string]: Input | Output;
}

type NamedVariable = (Input | Output) & { name: string };
```

이러한 타입은 인터페이스로 표현될 수 없다. 타입은 일반적으로 인터페이스보다 할 수 있는 것들이 많다. 타입은 union이 될 수 있고 또한 mapped 혹은 조건 타입과 같은 특징들에 대하여도 적용할 수 있다. 쉽게 튜플이나 배열 타입도 표현할 수 있다.

```typescript
type Pair = [number, number];
type StringList = string[];
type NamedNums = [string, ...number[]];
```

이를 인터페이스로 튜플로 만든다면 아래와 같다.

```typescript
interface Tuple {
  0: number;
  1: number;
  length: 2;
}

const t: Tuple = [10, 20]; // OK
```

이러한 표현식은 매우 어색하고 튜플이 가진 기능들을 버리게 되기에 인터페이스보다 타입을 쓰는 것이 좋다.

그러나, 인터페이스는 타입이 가지지 못한 몇몇 기능들이 존재한다. 이들 중하나는 인터페이스는 `augmented(증강된)` 특성이다. State 예제를 통하여 우리는 아래와 같이 확장할 수 있다.

```typescript
interface IState {
  name: string;
  capital: string;
}

interface IState {
  population: number;
}

const wyoming: IState = {
  name: "Wyoming",
  capital: "Cheyenne",
  population: 500_000,
}; // OK
```

이를 `declaration merging (합병 선언)`이라고 한다.

이제 처음에 던진 질문으로 돌아와서 얘기를 하자면 복잡한 타입을 쓴다면 무조건 타입을 쓴다. 하지만 간단한 객체 타입이라면 우리는 `consistency`와 `augmentation`을 고려해야 한다. 이 때는 기존 코드가 interface 를 일관적으로 사용하며 구성되어 있다면 interface를 쓰고 아니면 type을 쓰면 된다. 아직 코드가 짜여지지 않는 상태라면 API를 고려하여 새로운 타입도 확장하기 위해서는 인터페이스가 좋다. 그러나 타입이 내부적으로 사용된다면 `declaration merging (합병 선언)`은 실수가 될 가능성이 있기에 타입을 사용한다.

### 기억해야할 것!!

1. 타입과 인터페이스의 유사점과 차이점을 이해해야 한다.
2. 각 구문을 사용하며 같은 타입을 작성하는 법을 알아야 한다.
3. 프로젝트에서 선택을 결정할 때, 기존 코드 스타일과 `augmentation`가 이득이 될지 고려해야 한다.
