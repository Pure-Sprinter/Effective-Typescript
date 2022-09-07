## Item 20: Use Different Variables for Different Types

자바스크립트에서 다른 타입의 값을 다른 목적으로 재사용하는 것은 문제가 없다.

```javascript
let id = "12-34-56";
fetchProduct(id); // Expects a string
id = 123456;
fetchProductBySerialNumber(id); // Expects a number
```

타입스크립트 상에서는 2개의 오류가 발생을 하게 된다.

```typescript
let id = "12-34-56";
fetchProduct(id);
id = 123456;
// ~~ '123456' is not assignable to type 'string'.
fetchProductBySerialNumber(id);
// ~~ Argument of type 'string' is not assignable to
//    parameter of type 'number'
```

변수의 값은 변경될 수 있지만, 변수의 유형은 일반적으로 변경되지 않다. 유형이 변경될 수 있는 일반적인 한 가지 방법은 좁혀지는 것이지만, 이는 유형이 새로운 값을 포함하도록 확장되지 않고 작아지는 것을 수반한다.

위 오류를 어떤식으로 고칠 수 있을까 아래처럼 string | number 를 하나의 타입으로 결합하면 된다. 그러나 더 나은 방식은 새로운 변수를 선언하는 것이다.

```typescript
let id: string | number = "12-34-56";
fetchProduct(id);

id = 123456; // OK
fetchProductBySerialNumber(id); // OK

const serial = 123456; // OK
fetchProductBySerialNumber(serial); // OK
```

두 변수가 있는 버전이 더 나은 이유는 여러 가지가 있다.

1. 관련 없는 두 개념(ID 및 일련 번호)을 분리한다.
2. 이를 통해 보다 구체적인 변수 이름을 사용할 수 있다.
3. 그것은 유형 추론을 개선한다. 형식 주석이 필요하지 않다.
4. 따라서 더 단순한 유형이 생성된다.
5. 이 옵션을 사용하면 변수 const를 허용하지 않고 선언할 수 있다. 이를 통해 사람과 유형 검사기가 추론하기 쉬워진다.

### 기억해야할 것!!

1. 한 변수의 값이 바뀌지만 유형은 일반적으로 잘 바뀌지 않는다.
2. 독자와 type checker를 에게 혼란을 주지 않기 위해서 다른 유형의 값을 가진 변수를 재사용하는 것을 피하라.
