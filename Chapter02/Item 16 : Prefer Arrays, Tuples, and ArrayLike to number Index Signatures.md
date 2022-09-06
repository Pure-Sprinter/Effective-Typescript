## Item 16: Prefer Arrays, Tuples, and ArrayLike to number Index Signatures

자바스크립트의 객체는 파이썬이나 자바와 같은 "hashable" 객체라는 개념이 존재하지 않는다. 만약 당신이 복잡한 객체를 키로 하게 되면 toString()이 호출되어 문자로 바뀌게ㅜ 된다. 그래서 숫자도 키가 될 수 없다.

```javascript
> x = {}
{}

> x[[1, 2, 3]] = 2
2

> x
{ '1,2,3': 1 }

> { 1: 2, 3: 4}
{ '1': 2, '3': 4 }
```

그렇다면 배열은 어떨까?? 배열도 객체이다. 따라서 배열의 객체가 가진 키를 살펴보게 되면 모두 문자열로 되어 있음을 알 수 있다.

```javascript
> x = [1, 2, 3]
[ 1, 2, 3 ]

> Object.keys(x)
[ '0', '1', '2' ]
```

타입스크립트는 숫자 키를 허용하고 이런 키와 문자열을 구분하려고 노력한다.

```typescript
interface Array<T> {
  // ...
  [n: number]: T;
}
```

그러나 이는 타입스크립트 런타임에 지워지게 된다. 왜냐하면 타입스크립트는 실행 결과는 결국 자바스크립트로 해석되기 때문에 위 타입은 허구라고 볼 수 있다.

```javascript
const keys = Object.keys(xs); // Type is string[]
for (const key in xs) {
  key; // Type is string
  const x = xs[key]; // Type is number
}
```

만약에 숫자 요소를 인덱스로 받고자 한다면 Array 를 사용해보면 숫자를 주는 것을 알 수 있다.

```javascript
for (const x of xs) {
  x; // Type is number
}
xs.forEach((x, i) => {
  i; // Type is number x; // Type is number
});
```

### 기억해야할 것!!

1. 배열도 객체라서 배열의 키값도 숫자가 아닌 문자열임을 이해해야 한다. 이러한 인덱스는 순수하게 타입스크립트가 버그를 잡아내기 위해 설계한 것이다.
2. 인덱스로 숫자를 사용하기 보다 Array, tuple, ArrayLike 타입을 선호한다.
