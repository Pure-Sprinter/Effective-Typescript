## Item 6: Use Your Editor to Interrogate and Explore the Type System

타입스크립트는 코드를 생성하지만 타입 시스템은 메인 이벤트이다. 이번 TypeScript's Type System 은 어떻게 생각하고 사용해야하는지 또한 어떤 선택을 해야 하는지에 대한 얘기를 하게 된다.

만약 우리가 타입스크립트를 설치하게 되면, 우리는 2개의 실행 요소를 가지게 된다.

- tsc, 타입스크립트 컴파일러
- tsserver, 타입스크립트 독립 서버

당신은 TypeScript 컴파일러를 직접 실행할 가능성이 훨씬 높지만, 서버는 언어 서비스를 제공하기 때문에 모든 면에서 중요하다. 여기에는 자동 완성, 검사, 탐색 및 리팩토링이 포함된다.

편집기에서 유형 오류를 보는 것도 유형 시스템의 뉘앙스를 배우는 좋은 방법이 될 수 있습니다. 예를 들어 이 함수는 ID로 HTML 요소를 가져오거나 기본 요소를 반환합니다. TypeScript는 다음 두 가지 오류를 표시합니다.

```typescript
function getElement(elOrId: string | HTMLElement | null): HTMLElement {
  if (typeof elOrId === "object") {
    return elOrId;
    // ~~~~~~~~~~~~~~ 'HTMLElement | null' is not assignable to 'HTMLElement'
  } else if (elOrId === null) {
    return document.body;
  } else {
    const el = document.getElementById(elOrId);
    return el;
    // ~~~~~~~~~~ 'HTMLElement | null' is not assignable to 'HTMLElement'
  }
}
```

if 문의 첫 번째 분기의 목적은 객체, 즉 HTML 요소까지 필터링하는 것이다. 그러나 이상하게도 자바스크립트에서 null의 유형은 "객체"이므로, eLORId는 여전히 해당 분기에서 null일 수 있다. Null 체크를 먼저 하면 이 문제를 해결할 수 있다. 두 번째 오류는 document.getElementById가 null을 반환할 수 있기 때문에 예외를 발생시켜 해당 사례도 처리해야 한다.

### 기억해야할 것!!

1. 타입스크립트 언어 서비스에서 에디터가 제공하는 이점을 잘 파악하고 사용하자.
2. 에디터를 사용하여 어떻게 타입 시스템이 작동하고 타입을 추론하는지에 대한 직관성을 얻을 수 있다.
3. 형식 선언 파일로 이동하여 동작을 모델링하는 방법을 알 수 있다.
