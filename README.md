# Effective-TypeScript

> Effective-TypeScript를 읽고 정리하는 공간입니다.

## 목표

5주 안에 Effective-TypeScript를 전부 다 읽고 이해하기

### 실행 가능한지 판단하기

1. 예상 완독 기간 : 5주 (8/31~10/11)
2. 1주 당 소화해야 하는 분량 : 62 / 5 = 12.4개
3. 하루 당 공부해야 하는 분량 : 12.4 / 5 = 약 2개 ~ 3개
4. 1개 당 소요되는 예상 시간 : 30분 ~ 1시간
5. 매일 투자해야 하는 예상 시간 : 1시간 ~ 3시간 => 최대 2시간으로 맞춰놓기(12시 ~ 2시)

## 목차

### 1. Getting to Know TypeScript

| 제목   | 내용                                                          | 설명 | 공부 날짜 | 학습 시간 |
| ------ | ------------------------------------------------------------- | ---- | --------- | --------- |
| Item 1 | Understand the Relationship Between TypeScript and JavaScript |      |           |           |
| Item 2 | Know Which TypeScript Options You're Using                    |      |           |           |
| Item 3 | Understand That Code Generation Is Independent of Types       |      |           |           |
| Item 4 | Get Comfortable with Structural Typing                        |      |           |           |
| Item 5 | Limit Use of the any Type                                     |      |           |           |

### 2. TypeScript's Type System

| 제목    | 내용                                                                  | 설명 | 공부 날짜 | 학습 시간 |
| ------- | --------------------------------------------------------------------- | ---- | --------- | --------- |
| Item 6  | Use Your Editor to Interrogate and Explore the Type System            |      |           |           |
| Item 7  | Think of Types as Sets of Values                                      |      |           |           |
| Item 8  | Know How to Tell Whether a Symbol Is in the Type Space or Value Space |      |           |           |
| Item 9  | Prefer Type Declarations to Type Assertions                           |      |           |           |
| Item 10 | Avoid Object Wrapper Types (String, number, Boolean, Symbol, BigInt)  |      |           |           |
| Item 11 | Recognize the Limits of Excess Property Checking                      |      |           |           |
| Item 12 | Apply Types to Entire Function Expressions When Possible              |      |           |           |
| Item 13 | Know the Differences Between type and interface                       |      |           |           |
| Item 14 | Use Type Operations and Generics to Avoid Repeating Yourself          |      |           |           |
| Item 15 | Use Index Signatures for Dynamic Data                                 |      |           |           |
| Item 16 | Prefer Arras, Tuples, and ArrayLike to number Index Signatures        |      |           |           |
| Item 17 | Use readonly to Avoid Errors Associated with Mutations                |      |           |           |
| Item 18 | Use Mapped Types to Keep Values in Sync                               |      |           |           |

### 3. Type Interface

| 제목    | 내용                                                           | 설명 | 공부 날짜 | 학습 시간 |
| ------- | -------------------------------------------------------------- | ---- | --------- | --------- |
| Item 19 | Avoid Cluttering Your Code with Inferable Types                |      |           |           |
| Item 20 | Use Different Variables for Different Types                    |      |           |           |
| Item 21 | Understand Type widening                                       |      |           |           |
| Item 22 | Understand Type Narrowing                                      |      |           |           |
| Item 23 | Create Objects All at Once                                     |      |           |           |
| Item 24 | Be Consistent in Your use of Aliases                           |      |           |           |
| Item 25 | Use async Functions Instead of Callbacks for Asynchronous Code |      |           |           |
| Item 26 | Understand How Context Is Used in Type Inference               |      |           |           |
| Item 27 | Use Functional Constructs and Libraries to Help Types Flow     |      |           |           |

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