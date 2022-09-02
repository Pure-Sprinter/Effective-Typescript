## Item 10: Avoid Object Wrapper Types (String, Number, Boolean, Symbol, BigInt)

자바스크립트는 7가지의 기본적인 값들을 가지는데 string, number, boolean, null, undefined, symbol, bigint 이다. 이러한 값들은 타입을 정의하는데 좋지만 여기에 속하는 편리한 방식들이 존재하지 않아 String object type을 정의하고 여러 기능을 집어넣었다. 예를 들어 String 기능 중 charAt 이 있다.

```typescript
// Don't do this!
const originalCharAt = String.prototype.charAt;
String.prototype.charAt = function (pos) {
  console.log(this, typeof this, pos);
  return originalCharAt.call(this, pos);
};

console.log("primitive".charAt(3));

// [String: 'primitive'] 'object' 3
// m
```

이 방식으로 생성된 this 값은 String object wrapper로 된 것으로 기본 타입이 아닌 Wrapper 타입으로 움직이게 된다. 이러한 Wrapper Type 은 우리에게 편리함을 제공해준다. 그러나 일반적으로 직접 인스턴스화할 이유는 없다. 타입스크립트는 string이 wrapper인지 primitive 인지 구별할 수 있는 타입을 제공해준다.

- string and String
- number and Number
- boolean and Boolean
- symbol and Symbol
- bigint and BigInt

타입으로서 제공을 해주지만 그에 따른 오류도 존재한다.

```typescript
function getStringLen(foo: String) {
  return foo.length;
}
getStringLen("hello"); // OK
getStringLen(new String("hello")); // OK

function isGreeting(phrase: String) {
  return ["hello", "good day"].includes(phrase); // ~~~~~~
  // Argument of type 'String' is not assignable to parameter
  // of type 'string'.
  // 'string' is a primitive, but 'String' is a wrapper object;
  // prefer using 'string' when possible
}
```

위 예시에서 알 수 있듯이 string 은 String 타입에 할당할 수 있지만 String 은 string 에 할당할 수 없어 오류가 발생하였다.

### 기억해야할 것!!

1. 어떻게 Object Wrapper type들이 Primitive 타입에게 method를 제공하는지 이해해야 한다. 또한 Wrapper 타입들을 인스턴스화 하고 직접적으로 사용하는 것을 피해야 한다.
2. 타입스크립트 Object Wrapper Type 을 피하고 Primitive 타입을 사용해야 한다.
