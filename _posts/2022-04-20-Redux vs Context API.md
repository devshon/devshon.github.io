---
layout: single
title: "Redux vs Context API"
categories: FrontEnd
tag: [total, redux, context]
---

# 리액트 전역상태관리

### 필요성

> 컴포넌트는 트리형태의 자료구조로 자식을 갖고 있는 구조이다.
>
> 그래서 props 를 넘기려면 2-3번 많으면 수도없이 자식을 거쳐서 가야한다. 그런데 중간을 거치는 props가 필요없는 자식은 컴포넌트는 props-drilling을 피해야한다.
>
> 그 문제를 해결하기 위한 방법으로 redux, context API, recoil, mobX 등을 활용할 수 있다.

![screencapture-0420528](/images/screencapture2.png)

### Redux vs Context API

> Context와 Redux는 다른 도구이며 다른 목적을 갖고있다.
>
> Redux는 상태관리의 목적으로 나타났고 패턴을 짜야하지만, Context API는 리액트 상태관리 훅과 조합하여 사용되었을 때 상태관리 도구로 활용할 수 있다.

### Choosing right tool

**Redux를 사용해야 할 때**

- UI 레이어와 분리된 복잡한 상태 관리 로직 작성이 필요할때
- Rudex 미들웨어 기능을 활용하거나 복잡한 비동기 작업으로 액션을 전달할 때
- 대향의 상태 값이 존재하고 업데이트 로직이 복잡하거나 거대한 코드 베이스를 여러 사람이 작업할 때

**Context API를 사용해야 할 때**

- 단순 props-drilling을 피해야 할 때
- 단순 값을 전달하는 파이프가 필요할 때
