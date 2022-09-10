## Item 22 : Understand Type Narrowing

확장의 반대는 축소로 타입스크립트가 넓은 범위의 타입을 좁은 범위로 바꾸는 작업을 말한다. 이에 대한 가장 흔한 케이스는 null 체크이다.

```typescript
const el = document.getElementById("foo"); // Type is HTMLElement | null
if (el) {
  el; // Type is HTMLElement
  el.innerHTML = "Party Time".blink();
} else {
  el; // Type is null
  alert("No element #foo");
}
```

우리는 위 코드를 아래와 같이 block 을 없애면서 작성할 수 있다. 타입스크립트가 타입을 블록과 결합하며 null 을 배제할 수 있다.

```typescript
const el = document.getElementById("foo"); // Type is HTMLElement | null
if (!el) throw new Error("Unable to find #foo");
el; // Now type is HTMLElement
el.innerHTML = "Party Time".blink();
```

우리는 타입의 범위를 좁히는 여러 방법들이 존재한다. instanceof 나 property 체크를 통하여 범위를 좁힐 수 있다.

```typescript
function contains(text: string, search: string | RegExp) {
  if (search instanceof RegExp) {
    search; // Type is RegExp
    return !!search.exec(text);
  }
  search; // Type is string
  return text.includes(search);
}

interface A {
  a: number;
}
interface B {
  b: number;
}
function pickAB(ab: A | B) {
  if ("a" in ab) {
    ab; // Type is A
  } else {
    ab; // Type is B
  }
  ab; // Type is A | B
}
```

타입스크립트는 위 코드 처럼 조건에 따라 타입을 추적하는 것을 잘한다. 그리고 타입을 확인하는 흔한 방법은 tag를 넣는 방식이다.

```typescript
interface UploadEvent {
  type: "upload";
  filename: string;
  contents: string;
}
interface DownloadEvent {
  type: "download";
  filename: string;
}

type AppEvent = UploadEvent | DownloadEvent;
function handleEvent(e: AppEvent) {
  switch (e.type) {
    case "download":
      e; // Type is DownloadEvent break;
    case "upload":
      e; // Type is UploadEvent break;
  }
}
```

위 형식을 "tagged union" or "discriminated union" 이라고 부른다. 만약 타입을 통해 해결할 수 없으면 우리가 직접적으로 함수를 만들어 타입의 범위를 좁힐 수 있다.

```typescript
function isInputElement(el: HTMLElement): el is HTMLInputElement {
  return "value" in el;
}

function getElementContent(el: HTMLElement) {
  if (isInputElement(el)) {
    el; // Type is HTMLInputElement
    return el.value;
  }
  el; // Type is HTMLElement
  return el.textContent;
}
```

이를 "user-defined type guard" 라고 볼 수 있다.

### 기억해야할 것!!

1. 타입스크립트가 조건문과 다른 타입들을 기반으로 타입의 범위를 좁히는 것을 이해하자.
2. tagged/discriminated union과 user-defined type guard를 사용하여 타입 범위를 좁히는데 도움을 주니 사용하자.
