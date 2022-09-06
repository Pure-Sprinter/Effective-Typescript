Item 17: Use readonly to Avoid Errors Associated with Mutation

우리가 배열의 합을 담당하는 함수를 만들고 순환한다고 생각을 해보며 아래와 같은 코드를 짰다고 해보자. 그러면 어떤 값이 나올 것인가? 최종 모습은 아래와 같다.

```javascript
function printTriangles(n: number) {
  const nums = [];
  for (leti = 0; i < n; i++) {
    nums.push(i);
    console.log(arraySum(nums));
  }
}

printTriangles(5);
0;
1;
2;
3;
4;
```

우리가 의도한대로 값이 나오지 않은 이유는 우리가 정의한 arraySum 함수가 배열에 영향을 끼쳐 빈 배열로 반환하기 때문이다.

```javascript
function arraySum(arr: number[]) {
  let sum = 0,
    num;
  while ((num = arr.pop()) !== undefined) {
    sum += num;
  }
  return sum;
}
```

이러한 함수로 인해 우리는 readonly 타입을 써서 제어를 할 수 있게 된다.

```typescript
function arraySum(arr: readonly number[]) {
  let sum = 0,
    num;
  while ((num = arr.pop()) !== undefined) {
    // 'pop' does not exist on type 'readonly number[]'
    sum += num;
  }

  return sum;
}
```

여기서 제어해주는 오류 메시지는 readonly number[] 가 제공해주는 것으로 일반 number[] 와는 다른 방식의 구별 점들이 존재한다.

- 요소를 읽을 수 있지만 쓸수는 없다.
- 길이를 읽을 수 있지만 세팅할 수 없다.
- 배열을 변경하기 위한 함수를 호출할 수 없다.

이렇게 타입스크립트의 readonly 타입을 선언하게 되면 여러 제약사항들이 생기게 된다. 이 제어를 타입스크립트 상에서 아래와 같이 발생을 한다.

- 타입스크립트는 함수 내부에 변수를 변경 안하는지 확인한다.
- 호출자는 함수가 인자를 변형시키지 않음을 보장한다.
- 호출자는 함수에 readonly 배열을 넘길지도 모른다.

따라서 위 arraySum 코드를 아래와 같이 하게 되면 결과가 의도한대로 나오게 된다.

```typescript
function arraySum(arr: readonly number[]) {
  let sum = 0;
  for (const num of arr) {
    sum += num;
  }
  return sum;
}

printTriangles(5);
0;
1;
3;
6;
10;
```

이렇게 우리가 변경이 발생하지 않도록 사이드 이펙트를 방지하려면 readonly 태그를 사용하면 가능하게 된다. 또한, readonly를 선언하면 변경 오류 전체를 잡을 수 있게 되기에 효과적이다.

그렇다면 우리가 readonly로 선언한 요소는 아예 변경을 할 수 없게 되는 것인가? 하면 그것은 또 아니다. 아래의 코드를 먼저 보자

```typescript
function parseTaggedText(lines: string[]): string[][] {
  const currPara: readonly string[] = [];
  const paragraphs: string[][] = [];
  const addParagraph = () => {
    if (currPara.length) {
      paragraphs.push(
        currPara
        // ~~~~~~~~ Type 'readonly string[]' is 'readonly' and
        //          cannot be assigned to the mutable type 'string[]'
      );
      currPara.length = 0; // Clear lines
      // ~~~~~~ Cannot assign to 'length' because it is a read-only
      // property
    }
  };

  for (const line of lines) {
    if (!line) {
      addParagraph();
    } else {
      currPara.push(line);
      // ~~~~ Property 'push' does not exist on type 'readonly string[]'
    }
  }
  addParagraph();
  return paragraphs;
}
```

여기서 기존에 currPara에 readonly string[]이 붙은 것을 알 수 있다. 자바스크립트 상에서 readonly 를 붙이지 않아도 const 로 선언되어 있기 때문에 오류가 나지 않고 실행이되어도 값을 변경되기 보다 [[][][]] 와 같이 나올 수 있게 된다 .

이를 변경하고 수정하기 위해서는 const 대신에 let 을 쓰면 된다.

```typescript
let currPara: readonly string[] = [];
// ...
currPara = []; // Clear lines
// ...
currPara = currPara.concat([line]);
```

이렇게 선언을 하게 되면 currPara 자체적으로 바꾸는 것은 허용이 되지 않지만 값을 크게 할당하는 방식을 사용하면 수정이 가능해진다. 또한 readonly 는 배열보다 객체에 변형을 할당하는 새로운 방식도 제안할 수 있게 된다.

```typescript
let obj: { readonly [k: string]: number } = {};
// Or Readonly<{[k: string]: number}
obj.hi = 45;
// ~~ Index signature in type ... only permits reading
obj = { ...obj, hi: 12 }; // OK
obj = { ...obj, bye: 34 }; // OK
```

### 기억해야할 것!!

1. 함수에서 매개 변수를 수정하지 않으면 readonly로 선언한다. 이를 통해 기능이 더 명확해지고 구현 시 의도하지 않은 변형을 방지할 수 있다.
2. 변환 오류를 방지하고 변환이 발생하는 위치를 찾으려면 readonly를 사용한다.
3. const와 readonly의 차이를 이해하기.
4. readonly가 얕다는 것을 이해하기.
