## Item 30: Don’t Repeat Type Information in Documentation

```typescript
/**
 * Returns a string with the foreground color.
 * Takes zero or one arguments. With no arguments, returns the
 * standard foreground color. With one argument, returns the foreground color
 * for a particular page.
 */
function getForegroundColor(page?: string) {
  return page === "login" ? { r: 127, g: 127, b: 127 } : { r: 0, g: 0, b: 0 };
}
```

위 예시의 문제점은 코드와 주석의 내용이 일치하지 않는다는 점에 존재한다. 구체적으로 어떤 문제점이 있을까?

- 함수가 실제로 {r, g, b} 개체를 반환할 때 색상을 문자열로 반환한다고 한다.
- 이 함수는 형식 서명에서 이미 명확한 0 또는 1개의 인수를 사용한다고 설명한다.
- 불필요하게 말이 많다: 코멘트가 기능 선언 및 구현보다 길다!

타입스크립트의 타입 주석 시스템은 컴팩트하고 설명적이며 읽기 쉽도록 설계되었다. 따라서 주석으로 설명하는 것보다 훨씬 낫다. 따라서 아래와 같이 수정하는 것이 낫다.

```typescript
/** Get the foreground color for the application or a specific page. */
function getForegroundColor(page?: string): Color {
  // ...
}
```

### 기억해야할 것!!

1. 주석 및 변수 이름에 타입 정보가 반복되지 않도록 하기
2. 단위가 유형(예: 시간 Ms 또는 온도 C)에서 명확하지 않은 경우 변수 이름에 포함시키는 것에 대해 고려하기
