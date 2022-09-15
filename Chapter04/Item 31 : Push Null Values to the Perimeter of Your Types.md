## Item 31: Push Null Values to the Perimeter of Your Types

엄격한 Null Checks를 처음 켤 때 코드 전체에서 null 및 정의되지 않은 값을 확인하는 if 문 점수를 추가해야 하는 것처럼 보일 수 있다. 변수 A가 Null이 아닌 경우 변수 B도 Null이 아니며 그 반대도 Null이 아니라는 것을 알 수 있다. 이러한 암묵적 관계는 코드의 인간 독자와 유형 검사자 모두에게 혼란을 준다. 값은 혼합이 아닌 완전히 null이거나 완전히 null이 아닐 때 더 쉽게 사용할 수 있다.

```typescript
function extent(nums: number[]) {
  let min, max;
  for (const num of nums) {
    if (!min) {
      min = num;
      max = num;
    } else {
      min = Math.min(min, num);
      max = Math.max(max, num);
      // max Argument of type 'number | undefined' is not
      //     assignable to parameter of type 'number'
    }
  }
  return [min, max];
}
```

코드 상에서 min 타입을 확인하는 로직이 발생을 하게 된다 그러나 이것을 실행하게 되면 max에 대한 타입이 명확하지 않아 오류가 발생하게 된다. 이 코드는 결과적으로 `(number | undefined)[]`를 반환하게 된다. 따라서 둘다 체크하는 로직을 넣을 필요가 있다.

```typescript
function extent(nums: number[]) {
  let result: [number, number] | null = null;
  for (const num of nums) {
    if (!result) {
      result = [num, num];
    } else {
      result = [Math.min(num, result[0]), Math.max(num, result[1])];
    }
  }
  return result;
}
```

이 형식의 결과는 `[number, number] | null`로 작업하기가 훨씬 쉬워지고 한 번의 null 체크만 하면 된다.

### 기억해야할 것!!

1. null이거나 null이 아닌 값이 null 이거나 null이 아닌 다른 값과 암시적으로 엮이는 설계를 피하기
2. 더 큰 개체를 null 또는 완전히 null이 아닌 개체로 만들어 null 값을 설계에 추가하기
3. Null이 아닌 완전 클래스를 만들고 모든 값을 사용할 수 있을 때 구성하는 것이 좋다.
4. strictNullChecks는 코드의 많은 문제에 플래그를 표시할 수 있지만 null 값과 관련된 함수의 동작을 나타내는 데 필수적이다.
