## Item 11: Recognize the Limits of Excess Property Checking

우리가 Object 그자체로 선언된 타입으로 변수에 할당을하면 타입스크립트는 그 타입의 속성을 보장한다.

```typescript
interface Room {
  numDoors: number;
  ceilingHeightFt: number;
}

const r: Room = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: "present",
  // ~~~~~~~~~~~~~~~~~~ Object literal may only specify known properties,
  // and 'elephant' does not exist in type 'Room'
};
```

반대로 object를 선언하고 Room 타입을 할당하면 그 때는 성공이 되는데 그 이유는 할당한 값들이 Room 타입이 가지고 있는 요소의 하위 집합으로 포함하고 있기 때문이다 .

```typescript
const obj = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: "present",
};

const r: Room = obj; // OK
```

그렇다면 이 두 가지 예시는 무엇이 다를까? 처음에 우리는 구조 유형 시스템이 놓칠 수 있는 중요한 종류의 오류를 포착하는 데 도움이 되는 "excess property checking(과도한 속성 검사)" 라고 알려진 프로세스를 촉발했다. 그러나 이 과정에는 한계가 있으며, 이를 정기적인 할당 가능성 검사와 결합하면 구조 타이핑에 대한 직관력을 구축하는 것이 더 어려워질 수 있다. 이 코드는 런타임에 어떠한 종류의 오류도 던지지 않는다. 그러나 TypeScript가 말하는 것과 같은 정확한 이유로 당신이 의도한 대로 하지 않을 것 같다: 다크 모드가 아니라 다크 모드? 이다.

```typescript
interface Options {
  title: string;
  darkMode?: boolean;
}
function createWindow(options: Options) {
  if (options.darkMode) {
    setDarkMode();
  }
  // ...
}
createWindow({
  title: "Spider Solitaire",
  darkmode: true,
  // ~~~~~~~~~~~~~ Object literal may only specify known properties, but
  //               'darkmode' does not exist in type 'Options'.
  // Did you mean to write 'darkMode'?
});
```

순수하게 구조 type checker는 Options 유형의 도메인이 매우 광범위하기 때문에 이러한 종류의 오류를 발견할 수 없다. 문자열인 제목 속성을 가진 모든 개체와 다른 속성을 포함하지만 단, 이러한 개체가 true 또는 false가 아닌 다른 속성으로 설정된 다크 모드 속성을 포함하지 않는 한 해당 개체를 포함한다.

```typescript
const o1: Options = document; // OK
const o2: Options = new HTMLAnchorElement(); // OK
```

Excess Property Checking은 근본적인 구조 속성을 해치지 않으려 하는데 document와 HTMLAnchorElement는 title 이라는 속성을 가지고 있기도 하고 객체 리터럴이 아니기에 검사를 유발하지 않는다 하지만 만약 다른 타입으로 할당을 하면 아래와 같이 오류가 발생한다.

```typescript
const o: Options = { darkmode: true, title: "Ski Free" };
// ~~~~~~~~ 'darkmode' does not exist in type 'Options'...
```

과도한 속성 검사는 구조 타이핑 시스템에 의해 허용될 수 있는 속성 이름의 오타 및 기타 실수를 포착하는 효과적인 방법이다. 그러나 그것은 또한 범위가 매우 제한적이다: 그것은 대상 리터럴에만 적용된다. 이러한 한계를 인식하고 초과 속성 검사와 일반 유형 검사를 구분할줄 알아야 한다.

### 기억해야할 것!!

1. 우리가 Object Literal 을 변수에 할당하고 함수에 인자로 넘겨줄 때 Excess Property Checking 이 일어난다.
2. Excess Property Checking 은 오류를 찾는데 효과적인 방법이지만 타입스크립트가 수행하는 구조 할당가능성 검사와는 다릅니다.
3. Excess Property Checking 의 한계를 인지하기: 중간에 변수를 도입하면 이러한 검사가 제거된다.
