# Chapter 3 - Type Inference 타입 추론

타입스크립트는 극단적인 타입 추론을 사용한다. 잘 사용하면 우리 코드에 타입 안전을 얻기 위해 필요한 타입 주석 양을 크게 줄일 수 있다. 이 챕터에서는 타입 추론으로 발생하는 문제와 어떻게 고치는지 그리고 타입스크립트의 타입 추론에 대한 이해를 잘 알려줄 것이다.

## Item 19 : Avoid Cluttering Your Code with Inferable Types

자바스크립트를 사용하다가 타입스크립트로 넘어올 때 변수에 타입을 적게 되는데 이는 매우 비생산적이고 안좋은 스타일이다. 따라서 아래의 코드를 그 아래의 코드로 적는 것이 좋다.

```typescript
// before
let x: number = 12;

// after
let x = 12;
```

타입을 하나하나 다 적는 것은 불필요하다. 따라서 복잡한 객체도 타입을 선언하지 말고 바로 적는 것이 좋다.

```typescript
// before
const person: {
  name: string;
  born: {
    where: string;
    when: string;
  };
  died: {
    where: string;
    when: string;
  };
} = {
  name: "Sojourner Truth",
  born: {
    where: "Swartekill, NY",
    when: "c.1797",
  },
  died: {
    where: "Battle Creek, MI",
    when: "Nov. 26, 1883",
  },
};

// after
const person = {
  name: "Sojourner Truth",
  born: {
    where: "Swartekill, NY",
    when: "c.1797",
  },
  died: {
    where: "Battle Creek, MI",
    when: "Nov. 26, 1883",
  },
};
```
