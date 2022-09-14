## Item 28: Prefer Types That Always Represent Valid States

효율적인 타입 설계의 핵심은 유효한 상태만 보여주는 타입을 만드는 것이다. 예시를 들자면, 당신이 페이지를 고를 수 있는 웹앱을 만들고 보여준다고 하자. 그리고 상태는 아래와 같고 페이지를 렌더링하는 코드를 작성할 때 모든 필드를 고려할 필요가 있다.

```typescript
interface State {
  pageText: string;
  isLoading: boolean;
  error?: string;
}

function renderPage(state: State) {
  if (state.error) {
    return `Error! Unable to load ${currentPage}: ${state.error}`;
  } else if (state.isLoading) {
    return `Loading ${currentPage}...`;
  }
  return `<h1>${currentPage}</h1>\n${state.pageText}`;
}

async function changePage(state: State, newPage: string) {
  state.isLoading = true;

  try {
    const response = await fetch(getUrlForPage(newPage));
    if (!response.ok) {
      throw new Error(`Unable to load ${newPage}: ${response.statusText}`);
    }
    const text = await response.text();
    state.isLoading = false;
    state.pageText = text;
  } catch (e) {
    state.error = "" + e;
  }
}
```

페이지를 바꾸는 함수도 보게 되는데 이 때 많은 문제가 존재합니다.

- 상태 설정에서 로딩이 false로 잊었다.
- state.error를 삭제하지 않았기에 요청이 실패하면 메시지를 로드하기보다 오류 메시지가 계속 표시된다.
- 페이지가 로드되는 동안 사용자가 페이지를 변경하면 응답이 반환되는 순서에 따라 오류가 발생하면 뒷 페이지도 사라지게 된다.

문제는 state에 정보가 너무 적거나 많다는 것이다. State type을 사용하면 잘못된 상태를 나타내더라도 isLoading과 error를 모두 설정할 수 있다. 이렇게 하면 render()와 changePage()를 모두 제대로 구현할 수 없다. 따라서 아래와 같이 설정을 바꿀 수 있다.

```typescript
interface RequestPending {
  state: "pending";
}
interface RequestError {
  state: "error";
  error: string;
}
interface RequestSuccess {
  state: "ok";
  pageText: string;
}

type RequestState = RequestPending | RequestError | RequestSuccess;
interface State {
  currentPage: string;
  requests: { [page: string]: RequestState };
}

function renderPage(state: State) {
  const { currentPage } = state;
  const requestState = state.requests[currentPage];
  switch (requestState.state) {
    case "pending":
      return `Loading ${currentPage}...`;
    case "error":
      return `Error! Unable to load ${currentPage}: ${requestState.error}`;
    case "ok":
      return `<h1>${currentPage}</h1>\n${requestState.pageText}`;
  }
}

async function changePage(state: State, newPage: string) {
  state.requests[newPage] = { state: "pending" };
  state.currentPage = newPage;
  try {
    const response = await fetch(getUrlForPage(newPage));
    if (!response.ok) {
      throw new Error(`Unable to load ${newPage}: ${response.statusText}`);
    }
    const pageText = await response.text();
    state.requests[newPage] = { state: "ok", pageText };
  } catch (e) {
    state.requests[newPage] = { state: "error", error: "" + e };
  }
}
```

구현의 모호성은 완전히 사라졌다. 현재 페이지가 무엇인지 분명하고 모든 요청이 정확히 한 상태에 있다. 요청 후 사용자가 페이지를 변경해도 문제가 없고 이전 요청은 여전히 완료되지만 UI에는 영향을 주지 않는다.

### 기억해야할 것!!

1. 유효하거나 무효한 상태를 나타내는 타입들은 혼란스럽고 오류가 발생한 코드로 이어질 수 있다.
2. 유효한 상태만 나타내는 형식을 선호하기
