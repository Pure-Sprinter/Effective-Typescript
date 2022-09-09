# Effective-TypeScript

> Effective-TypeScript를 읽고 정리하는 공간입니다.

## 목표

5주 안에 Effective-TypeScript를 전부 다 읽고 이해하기

### 실행 가능한지 판단하기

1. 예상 완독 기간 : 5주 (8/29 ~ 9/30)
2. 1주 당 소화해야 하는 분량 : 62 / 5 = 12.4개
3. 하루 당 공부해야 하는 분량 : 12.4 / 5 = 약 2개 ~ 3개
4. 1개 당 소요되는 예상 시간 : 30분 ~ 1시간
5. 매일 투자해야 하는 예상 시간 : 1시간 ~ 3시간 => 최대 2시간으로 맞춰놓기(12시 ~ 2시)

## 목차

### 1. Getting to Know TypeScript

| 제목   | 내용                                                                                                                                                                                                                                   | 설명                                                                                                                                          | 공부 날짜  | 학습 시간 |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------- |
| Item 1 | [Understand the Relationship Between TypeScript and JavaScript](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter01/Item%201%20:%20Understand%20the%20Relationship%20Between%20TypeScript%20and%20JavaScript.md) | 자바스크립트에 비해 타입스크립트가 제공해주는 차별화 기능에 대한 개괄적인 내용                                                                | 8/29       | 45분      |
| Item 2 | [Know Which TypeScript Options You're Using](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter01/Item%202%20:%20Know%20Which%20TypeScript%20Options%20You're%20Using.md)                                         | noImplicitAny 와 strictNullChecks 설정이 타입스크립트에 끼치는 영향                                                                           | 8/29       | 30분      |
| Item 3 | [Understand That Code Generation Is Independent of Types](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter01/Item%203%20:%20Understand%20That%20Code%20Generation%20Is%20Independent%20of%20Types.md)           | 타입스크립트의 타입 시스템은 런타임 성능에 영향을 주지 않으며 이는 자바스크립트 코드로 생성하는 과정과 독립적이라는 것을 의미함               | 8/29, 8/30 | 45분      |
| Item 4 | [Get Comfortable with Structural Typing](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter01/Item%204%20:%20Get%20Comfortable%20with%20Structural%20Typing.md)                                                   | Structural Typing 은 duck type 을 모델링 하여 타입스크립트에서 만들어진 것으로 구조적 유사성을 활용하여 좀 더 원활한 코드를 작성할 수 있게 함 | 8/30       | 30분      |
| Item 5 | [Limit Use of the any Type](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter01/Item%205%20:%20Limit%20Use%20of%20the%20any%20Type.md)                                                                           | 타입스크립트의 any 유형을 쓰지 말아야 하는 이유                                                                                               | 8/30       | 20분      |

### 2. TypeScript's Type System

| 제목    | 내용                                                                                                                                                                                                                                                                   | 설명                                                                                                        | 공부 날짜 | 학습 시간 |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | --------- | --------- |
| Item 6  | [Use Your Editor to Interrogate and Explore the Type System](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%206%20:%20Use%20Your%20Editor%20to%20Interrogate%20and%20Explore%20the%20Type%20System.md)                                 | 타입스크립트를 사용할 때 에디터가 어떻게 타입 추론의 이점을 주고 기능을 제공하는지 정리한 것                | 8/31      | 15분      |
| Item 7  | [Think of Types as Sets of Values](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%207%20:%20Think%20of%20Types%20as%20Sets%20of%20Values.md)                                                                                           | 타입스크립트의 타입 시스템이 집합으로서 여겨지고 작동하는 것을 설명하는 내용                                | 8/31      | 35분      |
| Item 8  | [Know How to Tell Whether a Symbol Is in the Type Space or Value Space](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%208%20:%20Know%20How%20to%20Tell%20Whether%20a%20Symbol%20Is%20in%20the%20Type%20Space%20or%20Value%20Space.md) | type space와 value space 이 2가지 중 값들이 어디에 속하는지에 대한 설명을 해놓은 것                         | 8/31, 9/1 | 25분      |
| Item 9  | [Prefer Type Declarations to Type Assertions](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%209%20:%20Prefer%20Type%20Declarations%20to%20Type%20Assertions.md)                                                                       | Type Declaration(타입 선업)과 Type Assertion(타입 주입)의 차이점                                            | 9/1       | 20분      |
| Item 10 | [Avoid Object Wrapper Types (String, number, Boolean, Symbol, BigInt)](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2010%20:%20Avoid%20Object%20Wrapper%20Types.md)                                                                  | Object Wrapper Type 사용을 피해야하는 이유                                                                  | 9/2       | 20분      |
| Item 11 | [Recognize the Limits of Excess Property Checking](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2011%20:%20Recognize%20the%20Limits%20of%20Excess%20Property%20Checking.md)                                                          | Excess Property Checking 이 기존 타입스크립트의 Type Checker 와 어떻게 다른지 인지하고 사용하기             | 9/2       | 20분      |
| Item 12 | [Apply Types to Entire Function Expressions When Possible](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2012%20:%20Apply%20Types%20to%20Entire%20Function%20Expressions%20When%20Possible.md)                                        | 반복되는 함수식에 대하여 하나의 타입으로 변환하여 적용해보기                                                | 9/3       | 15분      |
| Item 13 | [Know the Differences Between type and interface](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2013%20:%20Know%20the%20Differences%20Between%20type%20and%20interface.md)                                                            | 타입과 인터페이스의 유사점과 차이점                                                                         | 9/3       | 20분      |
| Item 14 | [Use Type Operations and Generics to Avoid Repeating Yourself](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2014%20:%20Use%20Type%20Operations%20and%20Generics%20to%20Avoid%20Repeating%20Yourserlf.md)                             | 타입스크립트 상에서 코드를 줄이는 효과적인 방법                                                             | 9/4       | 30분      |
| Item 15 | [Use Index Signatures for Dynamic Data](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2015%20:%20Use%20Index%20Signatures%20for%20Dynamic%20Data.md)                                                                                  | 동적 타입에도 적용가능한 인덱스를 활용한 Generic Type 사용법                                                | 9/4       | 20분      |
| Item 16 | [Prefer Arrays, Tuples, and ArrayLike to number Index Signatures](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2016%20:%20Prefer%20Arrays%2C%20Tuples%2C%20and%20ArrayLike%20to%20number%20Index%20Signatures.md)                    | 숫자 인덱스 접근을 위해서 배열, 튜플과 같은 요소를 사용해야 하는 이유                                       | 9/6       | 30분      |
| Item 17 | [Use readonly to Avoid Errors Associated with Mutations](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2017%20:%20Use%20readonly%20to%20Avoid%20Errors%20Associated%20with%20Mutation.md)                                             | 함수가 받는 매개변수에 대하여 사이드 이펙트를 막는데 효과적으로 사용되는 방식인 readonly 에 대하여 이해하기 | 9/6       | 30분      |
| Item 18 | [Use Mapped Types to Keep Values in Sync](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter02/Item%2018%20:%20Use%20Mapped%20Types%20to%20Keep%20Values%20in%20Sync.md)                                                                          | 값을 동기화 시키기 위하여 Mapped Type을 사용해야 하는 이유                                                  | 9/7       | 20분      |

### 3. Type Interface

| 제목    | 내용                                                                                                                                                                                              | 설명                                                                     | 공부 날짜 | 학습 시간  |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ | --------- | ---------- |
| Item 19 | Avoid Cluttering Your Code with Inferable Types                                                                                                                                                   | 지나친 타입 작성을 피하며 잘 사용하기                                    | 9/9       | 10분(미완) |
| Item 20 | [Use Different Variables for Different Types](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter03/Item%2020%20:%20Use%20Different%20Variables%20for%20Different%20Types.md) | 같은 변수명을 사용하지 말고 다른 변수명과 다른 타입을 사용해야 하는 이유 | 9/7       | 20분       |
| Item 21 | [Understand Type widening](https://github.com/Pure-Sprinter/Effective-Typescript/blob/main/Chapter03/Item%2021%20:%20Understand%20Type%20Widening.md)                                                                                                                                                                          | 타입 확장을 적용하는 여러 방법들                                         | 9/9       | 20분       |
| Item 22 | Understand Type Narrowing                                                                                                                                                                         |                                                                          |           |            |
| Item 23 | Create Objects All at Once                                                                                                                                                                        |                                                                          |           |            |
| Item 24 | Be Consistent in Your use of Aliases                                                                                                                                                              |                                                                          |           |            |
| Item 25 | Use async Functions Instead of Callbacks for Asynchronous Code                                                                                                                                    |                                                                          |           |            |
| Item 26 | Understand How Context Is Used in Type Inference                                                                                                                                                  |                                                                          |           |            |
| Item 27 | Use Functional Constructs and Libraries to Help Types Flow                                                                                                                                        |                                                                          |           |            |

### 4. Type Design

| 제목    | 내용                                                         | 설명 | 공부 날짜 | 학습 시간 |
| ------- | ------------------------------------------------------------ | ---- | --------- | --------- |
| Item 28 | Prefer Types That Always Represent Valid States              |      |           |           |
| Item 29 | Be Liberal in What You Accept and Strict in What You Produce |      |           |           |
| Item 30 | Don't Repeat Type Information in Documentation               |      |           |           |
| Item 31 | Push Null Values to the Perimeter of Your Types              |      |           |           |
| Item 32 | Prefer Unions of Interfaces to Interfaces of Unions          |      |           |           |
| Item 33 | Prefer More Precise Alternatives to String Types             |      |           |           |
| Item 34 | Prefer Incomplete Types to Inaccurate Types                  |      |           |           |
| Item 35 | Generate Types from APIs and Specs, Not Data                 |      |           |           |
| Item 36 | Name Types Using the Language of Your Problem Domain         |      |           |           |
| Item 37 | Consider "Brands" for Nominal Typing                         |      |           |           |

### 5. Working with any

| 제목    | 내용                                                           | 설명 | 공부 날짜 | 학습 시간 |
| ------- | -------------------------------------------------------------- | ---- | --------- | --------- |
| Item 38 | Use the narrowest Possible Scope for any Types                 |      |           |           |
| Item 39 | Prefer More Precise Variants of any to Plain any               |      |           |           |
| Item 40 | Hide Unsafe Type Assertions in Well-Typed Functions            |      |           |           |
| Item 41 | Understand Evolving any                                        |      |           |           |
| Item 42 | Use unknown Instead of any for Values with and Unknown Type    |      |           |           |
| Item 43 | Prefer Type-Safe Approaches to Monkey Patching                 |      |           |           |
| Item 44 | Track Your Type Coverage to Prevent Regressions in Type Safety |      |           |           |

### 6. Types Declarations and @types

| 제목    | 내용                                                       | 설명 | 공부 날짜 | 학습 시간 |
| ------- | ---------------------------------------------------------- | ---- | --------- | --------- |
| Item 45 | Put TypeScript and @types in devDependencies               |      |           |           |
| Item 46 | Understand the Three Version Involved in Type Declarations |      |           |           |
| Item 47 | Export All Types that Appear in Public APIs                |      |           |           |
| Item 48 | Use TSDoc for API Comments                                 |      |           |           |
| Item 49 | Provide a Type for this in Callbacks                       |      |           |           |
| Item 50 | Prefer Conditional Types to Overloaded Declarations        |      |           |           |
| Item 51 | Mirror Types to Server Dependencies                        |      |           |           |
| Item 52 | Be Aware of the Pitfalls of Testing Types                  |      |           |           |

### 7. Writing and Running Your Code

| 제목    | 내용                                              | 설명 | 공부 날짜 | 학습 시간 |
| ------- | ------------------------------------------------- | ---- | --------- | --------- |
| Item 53 | Prefer ECMAScript Features to TypeScript Features |      |           |           |
| Item 54 | Know How to Iterate Over Objects                  |      |           |           |
| Item 55 | Understand the DOM hierarchy                      |      |           |           |
| Item 56 | Don't Rely on Private to Hide Information         |      |           |           |
| Item 57 | Use Source Maps to Debug TypeScript               |      |           |           |

### 8. Migrating to TypeScript

| 제목    | 내용                                                             | 설명 | 공부 날짜 | 학습 시간 |
| ------- | ---------------------------------------------------------------- | ---- | --------- | --------- |
| Item 58 | Write Modern JavaScript                                          |      |           |           |
| Item 59 | Use @ts-check and JSDoc to Experiment with TypeScript            |      |           |           |
| Item 60 | Use allowJs to Mix TypeScript and JavaScript                     |      |           |           |
| Item 61 | Convert Module by Module Up Your Dependency Graph                |      |           |           |
| Item 62 | Don't Consider Migration Complete Until You Enable noImplicitAny |      |           |           |
