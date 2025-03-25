## 📌State Hook


---
## ⚡️Effect Hook

데이터 가져오기, 구독(subscription) 설정하기, 수동으로 React 컴포넌트의 DOM을 수정하는 것까지 이 모든 것이 side effects입니다. 이런 기능들(operations)을 side effect(혹은 effect)라 부르는 것이 익숙하지 않을 수도 있지만, 아마도 이전에 만들었던 컴포넌트에서 위의 기능들을 구현해보았을 것입니다.

React 컴포넌트에는 일반적으로 두 종류의 side effects가 있습니다. 정리(clean-up)가 필요한 것과 그렇지 않은 것. 이 둘을 어떻게 구분해야 할지 자세하게 알아봅시다.


### useEffect
**`useEffect`가 하는 일은 무엇일까요?** useEffect Hook을 이용하여 우리는 React에게 컴포넌트가 렌더링 이후에 어떤 일을 수행해야하는 지를 말합니다. React는 우리가 넘긴 함수를 기억했다가(이 함수를 ‘effect’라고 부릅니다) DOM 업데이트를 수행한 이후에 불러낼 것입니다. 위의 경우에는 effect를 통해 문서 타이틀을 지정하지만, 이 외에도 데이터를 가져오거나 다른 명령형(imperative) API를 불러내는 일도 할 수 있습니다.

**`useEffect`를 컴포넌트 안에서 불러내는 이유는 무엇일까요?** `useEffect`를 컴포넌트 내부에 둠으로써 effect를 통해 `count` state 변수(또는 그 어떤 prop에도)에 접근할 수 있게 됩니다. 함수 범위 안에 존재하기 때문에 특별한 API 없이도 값을 얻을 수 있는 것입니다. Hook은 자바스크립트의 클로저를 이용하여 React에 한정된 API를 고안하는 것보다 자바스크립트가 이미 가지고 있는 방법을 이용하여 문제를 해결합니다.

**`useEffect`는 렌더링 이후에 매번 수행되는 걸까요?** 네, 기본적으로 첫번째 렌더링_과_ 이후의 모든 업데이트에서 수행됩니다.(나중에 [effect를 필요에 맞게 수정하는 방법](https://ko.legacy.reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)에 대해 다룰 것입니다.) 마운팅과 업데이트라는 방식으로 생각하는 대신 effect를 렌더링 이후에 발생하는 것으로 생각하는 것이 더 쉬울 것입니다. React는 effect가 수행되는 시점에 이미 DOM이 업데이트되었음을 보장합니다.