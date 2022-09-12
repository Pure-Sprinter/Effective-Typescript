## Item 24: Be Consistent in Your Use of Aliases

우리가 값에 이름을 작성할 때 아래와 같이 별칭(Alias)을 사용해서 나타내는 경우가 종종있다.

```typescript
const borough = { name: "Brooklyn", location: [40.688, -73.979] };
const loc = borough.location;

> loc[0] = 0;
> borough.location [0, -73.979]
```

별칭은 제어 흐름 분석을 어렵게 만들기 때문에 모든 언어에서 컴파일러 작성자의 골칫거리다. 별칭을 신중하게 사용하는 경우 TypeScript가 코드를 더 잘 이해하고 실제 오류를 더 많이 찾을 수 있다.

```typescript
interface Coordinate {
  x: number;
  y: number;
}
interface BoundingBox {
  x: [number, number];
  y: [number, number];
}
interface Polygon {
  exterior: Coordinate[];
  holes: Coordinate[][];
  bbox?: BoundingBox;
}
```

```typescript
function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  const box = polygon.bbox;
  if (polygon.bbox) {
    if (
      pt.x < box.x[0] ||
      pt.x > box.x[1] ||
      // ~~~ ~~~ Object is possibly 'undefined'
      pt.y < box.y[1] ||
      pt.y > box.y[1]
    ) {
      // ~~~ ~~~  Object is possibly 'undefined'
      return false;
    }
  }
}
```

```typescript
function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  polygon.bbox; // Type is BoundingBox | undefined
  const box = polygon.bbox;
  box; // Type is BoundingBox | undefined
  if (polygon.bbox) {
    polygon.bbox; // Type is BoundingBox box // Type is BoundingBox | undefined
  }
}
```

첫번째 예시와 2번째 예시 같은 경우에는 Null 체크를 하는 과정에서 코드를 다르게 짠 것이다. 둘다 오류가 발생하였고 이를 해결하려면 아래와 같이 코드를 짜야한다.

```typescript
function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  const box = polygon.bbox;
  if (box) {
    if (
      pt.x < box.x[0] ||
      pt.x > box.x[1] ||
      pt.y < box.y[1] ||
      pt.y > box.y[1]
    ) {
      // OK
      return false;
    }
  }
  // ...
}
```

여기서 보면 우리가 2개의 단어를 같은 것으로 사용하였다. bbox, box 둘다 있기 때문에 읽는 과정에서 헷갈릴 수 있다. 따라서 이를 통일된 별칭으로 사용하며 바꾸면 된다.

```typescript
function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  const { bbox } = polygon;
  if (bbox) {
    const { x, y } = bbox;
    if (pt.x < x[0] || pt.x > x[1] || pt.y < x[0] || pt.y > y[1]) {
      return false;
    }
  }
  // ...
}
```

### 기억해야할 것!!

1. 별칭은 타입스크립트가 타입을 좁히는 것을 막을 수 있다. 만약 당신이 변수에 대한 별칭을 만든다면 일관성있게 사용해라
2. 일관성있는 이름을 사용하려면 destructuring syntax 를 사용해라
3. 함수 호출이 속성의 형식 세분화를 무효화하는 방법을 알고 있어야 하고 속성보다 로컬 변수에 대한 세분화를 믿어라
