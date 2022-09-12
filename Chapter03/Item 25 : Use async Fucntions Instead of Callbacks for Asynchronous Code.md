## Item 25: Use async Functions Instead of Callbacks for Asynchronous Code

"pyramid of doom" : 고전 자바스크립트가 콜백을 사용할 때 동기적인 행동을 하는 것

```javascript
fetchURL(url1, function (response1) {
  fetchURL(url2, function (response2) {
    fetchURL(url3, function (response3) {
      // ...
      console.log(1);
    });
    console.log(2);
  });
  console.log(3);
});
console.log(4);

// Logs:
// 4
// 3
// 2
// 1
```

이처럼 코드가 거꾸로 실행되기에 읽기가 어려운 경우가 있다. ES2015, ES2017로 넘어가게 되면서 코드가 점점 간소화 되었고 async, await 형태가 나오게 되었다.

```typescript
async function fetchPages() {
  try {
    const response1 = await fetch(url1);
    const response2 = await fetch(url2);
    const response3 = await fetch(url3); // ...
  } catch (e) {
    // ...
  }
}
```

타입스크립트 상에서도 이러한 형태의 async/await를 사용할 수 있다. 왜 사용해야할까?

1. Promise는 callback 보다 사용하기 쉽다.
2. 타입들은 Promise를 통해 흐름을 잘 이해할 수 있다.

만약 위 코드를 병렬적으로 사용한다고 하면 Promise.all을 사용해 아래와 같이 사용할 수 있다.

```typescript
async function fetchPages() {
  const [response1, response2, response3] = await Promise.all([
    fetch(url1),
    fetch(url2),
    fetch(url3),
  ]);
  // ...
}

function fetchPagesCB() {
  let numDone = 0;
  const responses: string[] = [];
  const done = () => {
    const [response1, response2, response3] = responses;
    // ...
  };
  const urls = [url1, url2, url3];
  urls.forEach((url, i) => {
    fetchURL(url, (r) => {
      responses[i] = url;
      numDone++;
      if (numDone === urls.length) done();
    });
  });
}
```

### 기억해야할 것!!

1. 더 나은 구성 및 유형 흐름을 위해 콜백보다 Promise를 선호하기
2. async/await 방식을 선호하고 가능한 경우 원시 Promise을 기다리기
3. 함수가 Promise을 반환하면 async임을 선언하기
