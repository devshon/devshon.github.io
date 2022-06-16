---
layout: single
title: "React18 Suspense"
categories: FrontEnd
tag: [total, react, swr]
---

# Suspense, 나의 생각

> 리액트 18버전이 나오면서 크게 바뀐 것 중에 Suspense(서스펜스)는 유독 눈에 띄었다. 서스펜스는 컴포넌트에서 비동기식으로 데이터를 가져올 때 pending 상태일 때 동작하게 된다. 무엇을 동작하냐면, Spinner 또는 로딩이다.
>
> 기존 Promise all을 사용하거나 각각의 컴포넌트에서 생기는 비동기 데이터 처리를 묶어야하는 번거러움이 있었는데 서스펜스로 해당 컴포넌트를 전부 묶으면 다른 추가동작을 구현 할것도 없었다.

---

# Suspense 사용

### 진행상황

Todo List를 주제로 사용해봤다. 내가 만든 Todo List는 상단에 에디터로 등록 기능이 있고 하단엔 **진행중인 카드**와 **진행완료된 카드**로 나눠있다.

두 종류에 카드는 각각 컴포넌트를 분리했고 다른 api로 호출된다. 그렇기 때문에 데이터가 느릴경우 각자 출력되는 시간이 다를 것이고, 나는 각각의 리스트 컴포넌트에서 api를 호출하여 분리할 것이기 때문에 `Promise all`을 사용하기가 어렵다고 생각한다.

![screencapture-5364911](/images/screencapture-5364911.png)

### 서스펜스 응용

**📌 코드는 가독성을 위해서 불필요한 부분은 제거했습니다.**

우선 최상위 컴포넌트에 Suspense로 동시 로딩을 적용할 컴포넌트 범위까지 묶는다.

```tsx
import { Suspense } from "react";

const Todo = () => {
  return (
    <Suspense fallback={<h1>loading...</h1>}>
      <Editor />
      <List />
    </Suspense>
  );
};
```

Todo 컴포넌트 내부에 List 컴포넌트입니다. 아래코드를 보면 List 컴포넌트는 하위컴포넌트인 Item 컴포넌트를 갖고만 있을뿐 예외처리는 없습니다. 다만 Item 컴포넌트에서 진행중 | 완료된 카드를 구분할 수 있는 프로퍼티를 넘겨주었습니다.

```tsx
const List = () => {
  return (
    <div>
      <h1>Progress</h1>
      <Item type="progress" />
      <h1>Completed</h1>
      <Item type="completed" />
    </div>
  );
};
```

Item 컴포넌트에서는 SWR을 사용해서 넘어온 `type` 프로퍼티를 엔드포인트로 활용하여 각각 다른 api에서 데이터를 받아옵니다. 그리고 그 데이터는 **아무 예외처리를 하지 않고 컴포넌트에 데이터를 사용합니다.**

```tsx
import useSWR from "swr";
import { fetcher } from "../apis/fetcher";

const Item = ({ type }: { type: string }) => {
  const { data } = useSWR(`/todo/${type}`, fetcher);

  return (
    <>
      {data.todos.map((item: Todo) => {
        return (
          <div
            dangerouslySetInnerHTML={{
              __html: item.description,
            }}
          />
        );
      })}
    </>
  );
};
```

이 상태로 네트워크 설정을 느리게 한 뒤 새로고침을 하면 아래의 모습처럼 나옵니다.

![Jun-16-2022 16-56-27](/images/Jun-16-202216-56-27.gif)

---

# SWR로 서스펜스 사용

SWR을 사용한 이유는 mutate 기능을 사용해서 데이터 변경 시 자동 상태관리를 하기 위해서 입니다.

SWR에 서스펜스를 사용하는 방법은 간단합니다. `SWRConfig`를 사용합니다.

```tsx
import { SWRConfig } from "swr";

import Todo from "./components/Todo";

const App = () => {
  return (
    <>
      <SWRConfig
        value={{
          suspense: true,
        }}
      >
        <Todo />
      </SWRConfig>
    </>
  );
};

export default App;
```

### SWR을 사용한 간단한 Drag and Drop

![Jun-16-2022 17-12-00](/images/Jun-16-202217-12-00.gif)

---
