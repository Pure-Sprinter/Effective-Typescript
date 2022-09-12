## Item 27: Use Functional Constructs and Libraries to Help Types Flow

자바스크립트는 내장 정렬 함수를 포함하지 않았다. 그러나 시간이 지날 수록 함수형 프로그래밍 형식을 지원하고 람다 형식도 지원하며 여러 가지 라이브러리들도 계속 생겨나게 되었다. 예를 들어, map, reduce, lodash 등이 존재한다.

```typescript
const rows = rawRows
  .slice(1)
  .map((rowStr) =>
    rowStr
      .split(",")
      .reduce((row, val, i) => ((row[headers[i]] = val), row), {})
  );

import _ from "lodash";
const rows = rawRows
  .slice(1)
  .map((rowStr) => _.zipObject(headers, rowStr.split(",")));
```

이를 타입스크립트로 바꾸고 lodash를 적용한다면 타입에 대한 오류가 발생한다.

```typescript
const rowsB = rawRows.slice(1).map((rowStr) =>
  rowStr.split(",").reduce(
    (row, val, i) => ((row[headers[i]] = val), row),
    // ~~~~~~~~~~~~~~~ No index signature with a parameter of
    //                 type 'string' was found on type '{}'
    {}
  )
);

const rows = rawRows
  .slice(1)
  .map((rowStr) => _.zipObject(headers, rowStr.split(","))); // Type is _.Dictionary<string>[]
```

### 기억해야할 것!!

1. 내장된 함수형 프로그래밍이나 라이브러리들을 사용하면 중간 타입 체킹이 줄어들어 타입의 흐름을 자연스럽게 이어지게 하고 외부 타입 선언을 줄일 수 있다.
