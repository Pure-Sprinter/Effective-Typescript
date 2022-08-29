## Item 2 : Know Which TypeScript Options You're Using

아래의 코드가 형식 검사기를 통과할까?? 우리가 사용하는 옵션에 대한 이해 없이는 제대로 말을 할 수가 없다.

```typescript
function add(a, b) {
  return a + b;
}
add(10, null);
```

타입스크립트 컴파일러는 이러한 조합이 거의 100개 정도 존재한다. 이러한 설정들을 아래의 명령어로 확인할 수 있다.

```
$ tsc --noImplicitAny program.ts

// tsconfig.json으로도 확인이 가능하다.
{
    "compilerOptions": {
        "noImplicitAny": true
    }
}
```

대부분의 타입스크립트 구성 설정은 원본 파일을 찾는 위치와 생성되는 출력 유형을 제어합니다. 그러나 몇몇 사람들은 언어 자체의 핵심 측면을 통제한다. 이것들은 대부분의 언어가 사용자에게 맡기지 않는 높은 수준의 디자인 선택들이다. 타입스크립트는 구성 방식에 따라 매우 다른 언어로 느껴질 수 있다. 효과적으로 사용하려면 설정 요소 중 **noImplicitAny** 와 **strictNullChecks**와 같이 가장 중요한 것을 이해해야 한다.

noImplicitAny 는 변수에 알려진 유형이 있어야 하는지 여부를 제어합니다. 이러한 코드는 noImplicitAny 가 꺼져있을 때 유효합니다.

```typescript
function add(a, b) {
  return a + b;
}
```

만약 당신이 에디터 상에서 커서를 올리면 타입스크립트가 함수 유형에 대해 추론하는 결과를 나타내게 됩니다. `function add(a: any, b: any): any`와 같이 나오게 되는데 any 유형은 효과적으로 이러한 변수들이 포함된 코드의 형식 검사를 피하게 해줍니다. any는 유요한 도구지만 주의 깊게 사용해야 합니다.

당신이 any라고 적지 않았지만 여전히 위험한 유형으로 끝나기 때문에 이를 **암묵적 any**라고 부릅니다. 이는 당신이 noImplicitAny를 설정한다면 아래와 같이 에러가 됩니다.

```typescript
function add(a, b) {
  // ~ Parameter 'a' implicitly has an 'any' type // ~ Parameter 'b' implicitly has an 'any' type
  return a + b;
}
```

이러한 오류들은 유형 선언을 명시적으로 작성함으로써 수정이 됩니다. any나 이보다 더 명확한 유형을 넣으면 됩니다.

```typescript
function add(a: number, b: number) {
  return a + b;
}
```

타입스크립트는 유형 정보를 가질 때 가장 도움이 된다. 그래서 당신은 가능하다면 noImplicitAny 를 설정해야 확실하다. 일단 당신이 유형을 가진 모든 변수에 익숙해진다면 noImplicitAny 없는 타입스크립트는 좀 다른 언어라고 느낄 수 있을 것입니다.

새로운 프로젝트에서 당신은 타입이 있는 모든 변수를 작성하기 위해 noImplicitAny 를 세팅하고 시작해야 한다. 이것은 타입스크립트가 문제점을 발견하고 당신의 코드를 읽고 개발 경험을 향상시키는데 도움을 준다. noImplicitAny 를 끄도록 설정하는 것은 자바스크립트로부터 타입스크립트로 바꿀 때만 적절하다.

**strictNullChecks**는 모든 타입에서 null과 undefined가 허용되는 변수인지 벼루를 제어합니다. 아래의 코드는 strictNullChecks 가 꺼져있을 때 유효합니다.

```typescript
const x: number = null; // OK, null is a valid number
```

그러나 strictNullChecks 를 켜놓았을 때는 오류를 발생시킵니다.

```typescript
const x: number = null;
// ~ Type 'null' is not assignable to type 'number'
```

비슷한 오류는 null 대신 undefined 를 사용했을 때 발생할 것입니다. 만약 null 을 허용하고자 한다면 당신은 그 의도를 명확히 함으로써 오류를 수정할 수 있다.

```typescript
const x: number | null = null;
```

만약 당신이 null 을 허용하는 것을 원치 않는다면, 당신은 그게 어디서 오는지 추적하고 강제성을 넣는 것이 필요할 것이다.

```typescript
const el = document.getElementById("status");
el.textContent = "Ready"; // ~~ Object is possibly 'null'
if (el) {
  el.textContent = "Ready"; // OK, null has been excluded
}
el!.textContent = "Ready"; // OK, we've asserted that el is non-null
```

strictNullChecks 는 null 과 undefined 를 잡아내는데 매우 도움이 되지만 언어 사용의 어려움을 증가시킵니다. 만약 언어를 새롭게 사용하거나 자바스크립트 코드로 바꾼다면 strictNullChecks 를 꺼는 것은 선택이 될 수 있다. 당신이 strictNullChecks 를 설정하기 전에 noImplicitAny 를 확실히 설정해야 한다.

만약 당신이 strictNullChecks 없이 작업을 한다면 무서운 "undefined is not an object" 런타임 오류를 주시해야 한다. 이러한 모든 사항은 보다 엄격한 검사를 허용하는 것을 고려해야 한다는 점을 상기시켜 준다. 이 설정을 변경하면 프로젝트가 커질수록 더 어려워지므로 이 설정을 활성화하기 전에 너무 오래 기다리면 안된다.

### 기억해야할 것!!

1. 타입스크립트 컴파일러는 언어의 핵심 측면에 영향을 끼치는 몇 설정들을 포함한다.
2. 명령줄 옵션 대신 tsconfig.json을 사용하여 타입스크립트를 구성한다.
3. 자바스크립트 프로젝트를 타입스크립트로 전환하지 않는 경우 noImplicitAny 를 설정한다.
4. strictNullChecks 를 사용하여 "undefined is not an object" 스타일 런타임 오류를 방지한다.
