## Item 3 : Understand That Code Generation Is Independent of Types

고수준에서 tsc(TypeScript Compiler)는 2가지를 한다.

- tsc는 브라우저 상에서 작동하는 오래된 자바스크립트에서 다음 버전의 타입스크립트/자바스크립트로 변환한다.
- 타입 오류는 코드를 확인하며 진행된다.

놀라운 것은 이 2가지 행동은 전체적으로 서로 독립적이라는 것으로 다른 방법을 추가한다고 기존 유형이 타입스킄립트가 방출하는 자바스크립트에 영향을 끼칠수 없다. 실행되는 자바스크립트가 있기에, 이는 유형이 코드를 실행하는 방식에 영향을 끼치지 못함을 의미한다.

### Code with Type Errors Can Produce Output

코드 출력은 유형 거사에 독립적이기에 유형 오류를 가진 코드는 결과를 만듭니다.

```
$ cat test.ts
let x = 'hello';
x = 1234;

$ tsc test.ts
test.ts:2:1 - error TS2322: Type '1234' is not assignable to type 'string'
2 x = 1234; ~

$ cat test.js
var x = 'hello'; x = 1234;
```

C와 Java와 같이 유형 검사를 진행하는 요소에 대하여 놀랄 수 있다. 당신이 모든 타입스크립트 에러를 그런 언어의 경고와 비슷하게 고려 할 수 있다. 그것들은 문제가 될만한 부분을 알려주지만 빌드 과정을 멈추지 않는다.

> `Compiling and Type Checking`
> 오류가 있는 경우 코드 방출은 실제로 유용하다. 웹 응용 프로그램을 만드는 경우 특정 부분에 문제가 있다는 것을 알 수 있다. 그러나 타입스크립트는 오류가 있는 경우에도 코드를 생성하므로 수정하기 전에 응용 프로그램의 다른 부분을 테스트할 수 있다. 코드를 커밋할 때 예상 또는 예상치 못한 오류가 무엇인지 기억해야 하는 함정에 빠지지 않도록 0 오류를 목표로 해야 힌다. 오류에 대한 출력을 사용하지 않도록 설정하려면 tsconfig.json에서 noEmitOnError 옵션을 사용하거나 빌드 도구에서 해당 옵션을 사용할 수 있다.

### You Cannot Check TypeScript Types at Runtime

아래와 같이 코드가 있다고 가정하자

```typescript
interface Square {
  width: number;
}
interface Rectangle extends Square {
  height: number;
}
type Shape = Square | Rectangle;
function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    // ~~~~~~~~~ 'Rectangle' only refers to a type,
    //           but is being used as a value here
    return shape.width * shape.height;
    // ~~~~~~ Property 'height' does not exist
    //
  } else {
    return shape.width * shape.width;
  }
}
```

`instanceof` 체크는 런타임에 일어나지만 Rectangle은 하나의 유형으로 런타임 코드 과정에 영향을 미치지 못합니다. 타입스크립트 유형은 자바스크립트 컴파일 파트가 단순히 모든 인터페이스 타입, 타입 주석을 지우면서 삭제될 수 있다. 처리 중인 모양의 유형을 확인하려면, 당신은 런타임에 그 유형을 재구성할 방법이 필요하다. 이 아래 경우에 height 존재를 확인할 수 있다.

```typescript
function calculateArea(shape: Shape) {
  if ("height" in shape) {
    shape; // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape; // Type is Square
    return shape.width * shape.width;
  }
}
```

속성 검사에는 런타임에 사용할 수 있는 값만 포함되지만 유형 검사기는 모양의 유형을 직사각형으로 세분화할 수 있기 때문에 이 기능이 작동합니다. 다른 방식은 런타임에 이용할 수 있는 측면에서 유형을 명시적으로 저장하는 "tag" 기법이다.

```typescript
interface Square {
  kind: "square";
  width: number;
}
interface Rectangle {
  kind: "rectangle";
  height: number;
  width: number;
}
type Shape = Square | Rectangle;
function calculateArea(shape: Shape) {
  if (shape.kind === "rectangle") {
    shape; // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape; // Type is Square
    return shape.width * shape.width;
  }
}
```

여기서 Shape 타입은 "tagged union"의 한 예시가 될 수 있다. 왜냐하면 런타임에 유형을 확인하기 쉽기 때문에 타입스크립트 어디에나 있다.

몇몇 구조는 type과 value를 둘다 소개해준다. class 키워드가 그것으로 Square와 Rectangle 클래스들은 오류를 해결할 다른 방법을 가지고 있다.

```typescript
class Square {
  constructor(public width: number) {}
}

class Rectangle extends Square {
  constructor(public width: number, public height: number) {
    super(width);
  }
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    shape; // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape; // Type is Square
    return shape.width * shape.width; // OK
  }
}
```

참고) 클린 코드 상에서 타입을 체킹하기 보다 타입 체킹하기 전 플로우를 잘 개선하여 타입 체크 과정이 사라지도록 하는 것이 좋다고 했다.

### Type Operations Cannot Affect Runtime Values

당신이 문자열이나 숫자 값을 가지고 있다고 가정하자 그리고 당신은 그것을 항상 숫자로 평준화 한다고 하면 여기에 타입 확인을 진행하는 안좋은 가이드가 있다.

```typescript
function asNumber(val: number | string): number {
  return val as number;
}

// javascript
function asNumber(val) {
  return val;
}
```

`as number`은 type operation이다 그래서 런타임 코드 동작에 영향을 줄 수 없다. 값을 일반화하기 위하여 런타임 타입 시 확인하는 동작에는 자바스크립트 코드로 아래와 같이 표현할 수 있다.

```javascript
function asNumber(val: number | string): number {
  return typeof val === "string" ? Number(val) : val;
}
```

### Runtime Types May Not Be the Same as Declared Types

아래 함수는 최종적으로 console.log를 사용하게 될까??

```typescript
function setLightSwitch(value: boolean) {
  switch (value) {
    case true:
      turnLightOn();
      break;
    case false:
      turnLightOff();
      break;
    default:
      console.log(`I'm afraid I can't do that.`);
  }
}
```

타입스크립트는 보통 죽은 코드에 플래그를 붙이지만, 엄격한 옵션에도 불구하고 이에 대해 불평하지 않는다. 핵심은 boolean 이 선언된 유형이라는 것을 기억하는 것이다. TypeScript 유형이기 때문에 런타임에 사라진다. 자바스크립트 코드에서는 사용자가 실수로 "ON"과 같은 값을 가진 LightSwitch 를 호출할 수 있다.

순수한 TypeScript에서 이 코드 경로를 트리거하는 방법도 있다. 아마도 함수는 네트워크 호출에서 오는 값으로 호출될 수 있다.

```typescript
interface LightApiResponse {
  lightSwitchValue: boolean;
}
async function setLight() {
  const response = await fetch("/light");
  const result: LightApiResponse = await response.json();
  setLightSwitch(result.lightSwitchValue);
}
```

TypeScript는 런타임 유형이 선언된 유형과 일치하지 않을 때 매우 혼란을 겪고, 이러한 상황은 가능하면 피하는 것이 좋다. 그러나 선언한 값이 아닌 다른 유형을 가질 수 있다는 점을 명시해야 한다.

### You Cannot Overload a Function Based on TypeScript Types

C++과 같은 언어는 변수 타입마다 다른 함수의 복수 버전을 정의할 수 있다. 이를 "Overloading"이라고 하는데 타입스크립트 상에서는 코드가 타입과 독립되었기 때문에 이러한 구조는 불가능하다.

```typescript
function add(a: number, b: number) {
  return a + b;
} // ~~~ Duplicate function implementation
function add(a: string, b: string) {
  return a + b;
} // ~~~ Duplicate function implementation
```

타입스크립트는 Overloading 기능을 제공하지만 전체적으로는 type 수준에서만 이루어진다.

```typescript
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a, b) {
  return a + b;
}
const three = add(1, 2); // Type is number
const twelve = add("1", "2"); // Type is string
```

### TypeScript Types Have No Effect on Runtime Performance

types와 type operations들은 자바스크립틀 코드를 생성할 때 지워지기에 런타임 성능에 영향을 끼치지 않는다. 타입스크립트의 정적인 타입은 실제로 0에 가까운 비용이 든다.

여기에는 두 가지 주의 사항이 존재한다.

- 런타임 오버헤드는 없지만, 타입스크립트 컴파일러는 빌드 시간 오버헤드를 도입한다. 타입스크립트 팀은 컴파일러의 성능을 심각하게 받아들이고 있으며, 특히 증분 빌드의 경우 컴파일 속도가 상당히 빠르다. 오버헤드가 커지면 빌드 도구에서 유형 검사를 건너뛸 수 있는 "transpile only" 옵션을 사용할 수 있다.
- 타입스크립트는 이전 런타임 지원을 위해 내보내는 코드는 네이티브 구현에 비해 성능 오버헤드가 발생할 수 있습. 예를 들어 제너레이터 함수를 사용하고 제너레이터보다 이전인 ES5를 대상으로 하면 tsc가 작업을 수행하기 위해 일부 도우미 코드를 내보낸다. 이것은 발전기의 기본 구현과 비교하여 약간의 오버헤드가 있을 수 있다. 어쨌든, 이것은 방출 대상 및 언어 수준과 관련이 있으며 여전히 유형에 독립적이다.

### 기억해야할 것!!

1. 코드 생성기는 타입 시스템과 무관하다. 즉, 타입스크립트의 타입 선언 요소들이 코드 런타임 동작이나 성능에 영향을 주지 못한다는 의미를 말한다.
2. 타입 오류를 가진 프로그램이 컴파일 코드를 생성할 가능성이 존재한다.
3. 타입스크립트의 타입들은 런타임에 이용하지 않는다. 런타임에 타입을 쿼리하기 위해서 구조를 다시 짜는 노력이 필요하다. 그 과정에서 Tagged unions과 속성 체킹은 이를 수행하기 위한 흔한 방법이 된다. class와 같은 몇몇 구조는 런타임 시에 타입스크립트 type과 value를 둘다 이용한다.
