## Item 5: Limit Use of the any Type

타입스크립트 type system은 gradual, optional 하다. 이 단어가 함축하고 있는 의미로 당신이 bit by bit로 당신의 코드에 타입들을 더해주기 때문에 gradual 하고 당신이 원할 때마다 type checker 를 불능하게 만들 수 있기 때문에 optional 하다. 이러한 특징들이 가진 키가 바로 any type 이다.

```typescript
let age: number;
age = "12";
// ~~~ Type '"12"' is not assignable to type 'number'
age = "12" as any; // OK
```

### There's No Type Safety with any Types

앞선 예처럼 타입 선언 시 age를 number로 선언하였다. 하지만 any는 string 을 할당할 수 있게 해준다.

```typescript
age += 1; // OK; at runtime, age is now "121"
```

### any Lets You Break Contracts

우리가 함수를 작성할 때 함수의 타입을 지정하고 그 타입이 나오기를 기대하는 것과 같은 암묵적인 규칙이 있다. 그러나 any 타입은 이러한 타입의 제한과 관계없이 규칙을 벗어나 사용할 수 있게 해준다.

```typescript
function calculateAge(birthDate: Date): number {
  // ...
}

let birthDate: any = "1990-01-19";
calculateAge(birthDate); // OK
```

birthDate 라는 변수는 Date 라는 타입이어야 한다. any 타입으로 문자를 넣었는데 문제가 발생하지 않겠지만 추후 자바스크립트로 변환되는 과정에서 타입들 사이에서 충돌이 발생할 가능성이 있다.

### There Are No Language Services for any Types

우리가 Object 코드를 작성했다고 가정해보자. 일반적으로 Object 내 변수에 접근을 하면 자동으로 어떤 변수가 있는지 알려준다. 그러나 타입스크립트의 any 타입을 선언하면 자동으로 변수가 무엇이 있는지 알려주지 않는다.

또한, 이름을 변경하는 경우에도 어떤 이름으로 변경이 되었고 변수가 변경되었는지 알려주지 않는다.

```typescript
interface Person {
  first: string;
  last: string;
}

const formatName = (p: Person) => `${p.first} ${p.last}`;
const formatNameAny = (p: any) => `${p.first} ${p.last}`;
```

위 코드를 아래의 코드로 바꾼다고 치면 변수의 변화를 any 는 감지하지 못한다.

```typescript
interface Person {
  firstName: string;
  last: string;
}

const formatName = (p: Person) => `${p.firstName} ${p.last}`;
const formatNameAny = (p: any) => `${p.first} ${p.last}`;
```

이처럼 Services 란 코드를 작성하는 과정에서 생산성을 높이기 위한 변화 감지와 같은 서비스를 말하는 것이다.

### any Types Mask Bugs When You Refactor Code

우리가 웹앱을 만들때에 여러 종류의 아이템 중 하나를 사용자가 선택한다고 해보자. 그 때 컴포넌트들은 선택의 결과를 반환하는 callback 이 필요하게 되고 아래 처럼 코드를 작성할 수 있다.

```typescript
interface ComponentProps {
  onSelectItem: (item: any) => void;
}

function renderSelector(props: ComponentProps) {
  /* ... */
}

let selectedId: number = 0;

function handleSelectItem(item: any) {
  selectedId = item.id;
}

renderSelector({ onSelectItem: handleSelectItem });
```

나중에 전체 항목 개체를 onSelectItem 에 전달하기가 더 어려운 방식으로 작업한다. 하지만 ID 만 있으면 되니까 ComponentProps에서 서명을 변경한다.

```typescript
interface ComponentProps {
  onSelectItem: (id: number) => void;
}
```

구성 요소를 업데이트하면 모든 것이 type checker 를 통과한다. handleSelectItem 은 매개 변수를 사용하므로 ID 와 마찬가지로 항목에서도 만족한다. type checker 를 통과함에도 불구하고 런타임 예외를 생성한다. 좀 더 구체적인 유형을 사용했다면, type checker 에 의해 잡혔을 것이다.

### any Hides Your Type Design

애플리케이션 상태와 같은 복잡한 개체의 유형 정의는 상당히 길어질 수 있다. 페이지 상태에 있는 수십 개의 속성에 대한 유형을 작성하는 대신 아무 유형이나 사용하고 싶은 유혹이 존재한다.

이는 당신의 상태 설계를 숨기기 때문에 문제가 된다. 챕터 4에서 설명하듯이, 깨끗하고 올바르며 이해하기 쉬운 코드를 작성하기 위해서는 좋은 유형 설계가 필수적이다. 모든 유형의 경우 유형 설계는 암시적으로 그 디자인이 좋은 것인지, 심지어 그 디자인이 무엇인지 알 수 없게 만든다. 동료에게 변경 내용을 검토하도록 요청하면, 동료는 응용 프로그램 상태를 변경했는지 여부와 변경 방법을 재구성해야 하기에 모두가 볼 수 있도록 그것을 쓰는 것이 낫다.

### any Undermines Confidence in the Type System

여러분이 실수를 하고 유형 검사기가 그것을 잡을 때마다, 그것은 유형 시스템에 대한 여러분의 자신감을 높여준다. 그러나 런타임에 유형 오류가 발생하면, 그 자신감은 타격을 받고 더 큰 팀에 TypeScript를 도입하는 경우 동료들이 TypeScript를 사용할 가치가 있는지 의문을 가질 수 있다. any 유형은 발견되지 않은 오류의 원인이 되는 경우가 많다.

타입스크립트는 당신의 삶을 더 쉽게 만드는 것을 목표로 한다. 하지만 타입스크립트는 타입오류를 수정하면서도 머릿속에 있는 실제 타입들과 계속 비교해야 하기 때문에 타입이 많은 타입스크립트는 타입을 입력하지 않은 자바스크립트보다 작업하기가 더 어려울 수 있다. 유형이 실제로 일치하면 유형 정보를 머릿 속에 저장해야 하는 부담에서 벗어날 수 있다.

### 기억해야할 것!!

1. any 타입은 효과적으로 type checker와 타입스크립트 언어 서비스들이 작동되는지도 모르게 지나가게 한다. 이는 실제 문제를 감추고 개발자 경험을 해치고 타입 시스템에 대한 신뢰를 떨어뜨리기에 가능하면 any 타입 쓰는 것을 피해야 한다.
