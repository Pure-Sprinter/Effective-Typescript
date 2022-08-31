## Item 8: Know How to Tell Whether a Symbol Is in the Type Space or Value Space

타입스크립트 symbol은 둘 중 하나의 공간에 존재한다.

1. Type space
2. Value space

이 개념은 혼란을 가져온다. 왜냐하면 같은 이름을 가진 요소가 서로 다른 공간을 의존하고 있을 수 있기 때문이다.

```typescript
interface Cylinder {
  radius: number;
  height: number;
}

const Cylinder = (radius: number, height: number) => ({ radius, height });
```

Cylinder 인터페이스는 type space 내 symbol을 소개해준다.
