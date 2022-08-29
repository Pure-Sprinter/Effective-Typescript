## Item 3 : Understand That Code Generation Is Independent of Types

고수준에서 tsc(TypeScript Compiler)는 2가지를 한다.

- tsc는 브라우저 상에서 작동하는 오래된 자바스크립트에서 다음 버전의 타입스크립트/자바스크립트로 변환한다.
- 유형 오류는 코드를 확인합니다.

놀라운 것은이 2가지 행동은 전체적으로 서로 독립적이다. 다른 방법을 넣는다고 이 유형이 타입스킄립트가 방출하는 자바스크립트에 영향을 끼칠수 없다. 실행되는 자바스크립트가 있기에, 이는 유형이 코드를 실행하는 방식에 영향을 끼치지 못함을 의미한다.

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
