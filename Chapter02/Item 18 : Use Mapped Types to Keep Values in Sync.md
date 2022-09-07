## Item 18: Use Mapped Types to Keep Values in Sync

- 산점도를 그리기 위한 UI 성분을 쓰고 있다고 가정하면 아래와 같은 속성들이 있다.

```typescript
interface ScatterProps {
  // The data
  xs: number[];
  ys: number[];
  // Display
  xRange: [number, number];
  yRange: [number, number];
  color: string;
  // Events
  onClick: (x: number, y: number, index: number) => void;
}
```

불필요한 작업을 피하기 위해 필요할 때만 차트를 다시 그리려고 해서 데이터 또는 표시 속성을 변경하려면 다시 그리기가 필요하지만 이벤트 핸들러를 변경할 경우에는 다시 그리기가 필요하지 않다. 이러한 최적화 유형은 이벤트 핸들러 Prop가 모든 렌더에서 새 화살표 함수로 설정될 수 있는 React 구성 요소에서 일반적이다.

```typescript
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k]) {
      if (k !== "onClick") return true;
    }
  }
  return false;
}
```

만약 이 코드에 새로운 기능을 넣는다고 생각해봐라. 그러면 우리는 변경할 때마다 다시 그려야 하고 이를 보수적인 관점에서 "fail closed" 접근이라고 할 수 있다.

"fail open" 접근은 이런식으로 생겼다.

```typescript
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  return (
    oldProps.xs !== newProps.xs ||
    oldProps.ys !== newProps.ys ||
    oldProps.xRange !== newProps.xRange ||
    oldProps.yRange !== newProps.yRange ||
    oldProps.color !== newProps.color
    // (no check for onClick)
  );
}
```

"fail open" 방식을 사용하면 불필요하게 다시 그리지는 않지만 필수적이 요소가 빠지게 된다. 우리는 이러한 실패 요소들을 피하기 위해 최적화된 mapped type을 사용하여 아래처럼 그릴 수 있다.

```typescript
const REQUIRES_UPDATE: { [k in keyof ScatterProps]: boolean } = {
  xs: true,
  ys: true,
  xRange: true,
  yRange: true,
  color: true,
  onClick: false,
};

function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]) {
      return true;
    }
  }
  return false;
}
```

[k in keyof ScatterProps] 는 type checker 에게 REQUIRES_UPDATES 가 ScatterProps 와 같은 속성들을 가져야 한다고 말을 하게 된다.

### 기억해야할 것!!

1. mapped type 을 사용하여 관련된 값과 타입을 동기화시켜야 한다.
2. 인터페이스에 새 속성을 추가할 때 mapped type 을 사용하여 선택해보자.
