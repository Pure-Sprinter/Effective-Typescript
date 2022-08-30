## Item 4: Get Comfortable with Structural Typing

자바스크립트는 duck type 이라고 할 수 있다. 만약에 올바른 속성들을 가진 함수를 넘기면 당신이 만들어낸 값이 신경 쓰지 않을 것이다. 타입스크립트는 이러한 duck type 행동을 모델링하여 타입 검사기의 이해도가 우리가 신경쓰는 것 이상으로 커버해주기 때문에 놀랄 수도 있다. 좋은 구조의 타입을 가지는 것은 오류와 오류가 아닌 것에 대한 이해를 도와주고 보다 탄탄한 코드를 작성하도록 해준다.

2D vector 타입을 가지고 작성을 한다고 해보자. 그러면 우리는 길이를 다음과 같은 함수로 작성할 수 있을 것이다.

```typescript
interface Vector2D {
  x: number;
  y: number;
}

function calculateLength(v: Vector2D) {
  return Math.sqrt(v.x * v.x + v.y * v.y);
}

interface NamedVector {
  name: string;
  x: number;
  y: number;
}

const v: NamedVector = { x: 3, y: 4, name: "Zee" };
calculateLength(v); // OK, result is 5
```

여기서 신기한 것은 calculateLength 함수가 Vector2D 의 타입을 가진 변수를 받아야 하는데 NamedVector 도 작동을 했다는 것이다. 이는 타입스크립트의 타입 시스템이 NamedVector 의 구조가 Vector2D 와 양립할 수 있기 때문에 사용할 수 있게 했기 때문이다. 우리는 이것을 “structural typing” 이라는 단어로 말할 수 있다. 그러나 이것은 다음과 같은 구조에서 문제를 일으킬 여지가 있다.

```typescript
interface Vector3D {
  x: number;
  y: number;
  z: number;
}

function normalize(v: Vector3D) {
  const length = calculateLength(v);
  return {
    x: v.x / length,
    y: v.y / length,
    z: v.z / length,
  };
}

normalize({ x: 3, y: 4, z: 5 });
// { x: 0.6, y: 0.8, z: 1 }
```

여기서 문제가 발생하지 않는다. 3D 벡터를 넣었는데 왜 오류가 나지 않을까? z 성분이 작동 과정에서 무시가 되었고 type checker에서도 발견하지 않았다. 이는 타입스크립트의 유형 시스템이 좋든 실든 개방적으로 작동이 되기 떄문이다.

class와 같은 구조적인 타입핑은 효과적이다.

```typescript
class C {
  foo: string;
  constructor(foo: string) {
    this.foo = foo;
  }
}
const c = new C("instance of C");
const d: C = { foo: "object literal" }; // OK!
```

여기서 d가 C에게 할당이 되었는데 그것은 문자열인 foo 속성을 가지고 있기 때문이다. 구조적인 타이핑은 테스트를 작성할 때 도움이 된다. 데이터베이스에서 쿼리를 실행하고 결과를 처리하는 기능이 있다고 가정해 보면 아래와 같은 과정을 거치게 된다.

```typescript
interface Author {
  first: string;
  last: string;
}

function getAuthors(database: PostgresDB): Author[] {
  const authorRows = database.runQuery(`SELECT FIRST, LAST FROM AUTHORS`);
  return authorRows.map((row) => ({ first: row[0], last: row[1] }));
}

interface DB {
  runQuery: (sql: string) => any[];
}

function getAuthors(database: DB): Author[] {
  const authorRows = database.runQuery(`SELECT FIRST, LAST FROM AUTHORS`);
  return authorRows.map((row) => ({ first: row[0], last: row[1] }));
}
```

여기서 PostgresDB가 DB로 바뀌어도 해당 구조체가 가지고 있는 인터페이스와 기능이 매우 유사하기 때문에 함수를 실행하는데 문제가 없다.

따라서 아래와 같이 testcode를 작성하여도 구조적 타입핑(structural typing)이 제공해주는 기능으로 인해 쉽게 인터페이스 형식의 DB를 테스트 할 수 있다.

```typescript
test("getAuthors", () => {
  const authors = getAuthors({
    runQuery(sql: string) {
      return [
        ["Toni", "Morrison"],
        ["Maya", "Angelou"],
      ];
    },
  });
  expect(authors).toEqual([
    { first: "Toni", last: "Morrison" },
    { first: "Maya", last: "Angelou" },
  ]);
});
```

### 기억해야할 것!!

1. 자바스크립트의 duck type 을 이해하고 타입스크립트가 이를 모방하여 structural typing을 구축했음을 알아야 한다. 인터페이스에 할당된 값들은 타입 선언 리스트에 명백하게 속성을 가지게 된다.
2. class 도 structural typing 규칙을 따르는 것을 명심해야 한다.
3. 단위 테스트를 용이하게 하기 위한 structural typing 을 사용하자.
