---
title: React
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - React
  - 프론트엔드
  - UI
---

# React

## 개요

React는 사용자 인터페이스를 구축하기 위한 JavaScript 라이브러리이다. 컴포넌트 기반 아키텍처와 Virtual DOM을 활용하여 효율적인 UI 업데이트를 수행한다.

## 컴포넌트 정의 방식

React 컴포넌트를 정의하는 2가지 방식이 있다.

```js
// 1. Arrow Function
const Example = (props) => {
    return <div />
}

// 2. Function Declaration
function Example(props) {
    return <div />;
}
```

### 주요 차이점

| 구분 | Arrow Function | Function Declaration |
|------|---------------|---------------------|
| **this 바인딩** | 렉시컬 스코프의 this 사용 | 자신만의 this 컨텍스트 생성 |
| **호이스팅** | 호이스팅되지 않음 | 호이스팅됨 |
| **성능** | 차이 미미 | 차이 미미 |

현대 React 개발에서는 화살표 함수 스타일이 더 많이 사용되는 추세이다.

## Hook

### Hook 사용 규칙

1. **최상위(at the top level)**에서만 Hook을 호출해야 한다. 반복문, 조건문, 중첩된 함수 내에서 실행 금지.
2. **React 함수 컴포넌트** 내에서만 Hook을 호출해야 한다. 일반 JavaScript 함수에서는 호출 불가. (Custom Hook 내에서는 호출 가능)

### Effect Hook (useEffect)

`useEffect`를 이용하여 컴포넌트가 렌더링 이후에 수행해야 할 side effect를 정의한다. React는 전달된 함수(effect)를 기억했다가 DOM 업데이트 수행 후 호출한다.

Side effect의 두 종류:

**1. 정리(clean-up)가 필요한 경우 -- 함수를 반환**

```js
useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
});
```

**2. 정리가 필요 없는 경우 -- 반환값 없음**

```js
useEffect(() => {
    document.title = `You clicked ${count} times`;
});
```

`useEffect`를 컴포넌트 내부에 둠으로써 state 변수와 prop에 클로저를 통해 접근할 수 있다. 기본적으로 첫 번째 렌더링과 이후의 모든 업데이트에서 수행된다.

### Custom Hook

이름이 `use`로 시작하고, 내부에서 다른 Hook을 호출하는 함수를 Custom Hook이라 부른다. `useSomething` 네이밍 컨벤션은 linter 플러그인이 Hook을 인식하고 버그를 찾을 수 있게 해준다.

## React Fiber

### Reconciliation (재조정)

UI에서 변경 사항이 발생하면 **DOM과 Virtual DOM을 비교하여 변경사항을 식별하고 업데이트하는 과정**이다. 재조정과 렌더링은 `reconciler` 모듈과 `renderer` 모듈로 분리되어 실행된다.

### Fiber Reconciler

증분 렌더링(Incremental Rendering)을 구현하기 위해 도입된 아키텍처이다. 작업을 일시 정지하고 나중에 다시 시작할 수 있으며, 이전에 완료된 작업을 재사용하거나 필요하지 않은 경우 중단할 수 있다.

Fiber는 `current`와 `workInProgress` 두 가지 트리를 가지고 있다.

### 렌더 단계 (Render Phase)

- 비동기적으로 동작
- 두 Fiber 트리를 비교하고 변경된 Effect들을 수집
- React scheduler로 인해 허용되는 시간 동안 작업하고, user input이나 animation 같은 더 급한 작업이 있으면 메인 스레드를 양보
- `workInProgress` 트리의 변경사항은 언제든 버릴 수 있으므로, DOM 변경이나 생명주기 메서드는 이 단계에서 실행 불가

### 커밋 단계 (Commit Phase)

- 렌더 단계에서 수집한 Effect와 변경 정보를 가진 Fiber를 통해 Effect를 실행하고 DOM에 적용
- 동기적으로 한번에 이루어지며, 일시 정지나 취소 불가
- Commit 후 `workInProgress` 트리가 `current` 트리가 됨

## 관련 문서

- [[JavaScript]] - React의 기반 언어
- [[TypeScript]] - React와 함께 자주 사용되는 타입 시스템
- [[MARS-GUI]] - React 기반 GUI 프로젝트
