## ☑️[[Hook]] 사용 규칙
- **최상위(at the top level)** 에서만 Hook을 호출해야 한다. 반복문, 조건문, 중첩된 함수 내에서 실행 금지
- **React 함수 컴포넌트** 내에서만 Hook을 호출 해야한다. 일반 JavaScript 함수에서는 Hook을 호출해서는 안된다. (Custom Hook 내에서는 호출 가능함)
 
---

## `useEffect`

`useEffect`가 컴포넌트의 렌더링 이후에 다양한 side effects를 표현할 수 있음.
effect에 **정리(clean-up)** 가 필요한 경우에는 **함수를 반환**

```
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```

정리(clean-up)가 필요없는 경우에는 어떤 것도 반환하지 않는다.

```
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
```


---

## 💡 Custom Hook 
Custom Hook은 기능이라기보다는 컨벤션(convention)에 가깝습니다. 이름이 ”`use`“로 시작하고, 안에서 다른 Hook을 호출한다면 그 함수를 custom Hook이라고 부를 수 있습니다. `useSomething`이라는 네이밍 컨벤션은 **linter 플러그인이 Hook을 인식하고 버그를 찾을 수 있게 해줍니다.**

---
### React 컴포넌트를 정의하는 2가지 방식 

```js title:"Type1. use Arrow function"
const Example = (props) => {// do something 
	return <div/>
}
```

```js title:"Type2. use function"
function Example(props) { 
	// do something 
	eturn <div />; 
}
```

#### 1. 문법적 차이
- 첫 번째 예제는 ES6 화살표 함수(arrow function)를 사용합니다.
- 두 번째 예제는 전통적인 함수 선언식(function declaration)을 사용합니다.

#### 2. this 바인딩
- 화살표 함수는 자신만의 `this`를 생성하지 않고 렉시컬 스코프(lexical scope)의 `this`를 사용
- 일반 함수는 자신만의 `this` 컨텍스트를 생성
- React 함수형 컴포넌트에서는 일반적으로 `this`를 사용하지 않기 때문에 실제 기능 차이는 거의 없음

#### 3. 호이스팅(Hoisting)
- 함수 선언식은 호이스팅 되어 파일의 어디서든 접근할 수 있습니다.
- `const`로 정의된 화살표 함수는 호이스팅 되지 않으므로 선언 전에 사용할 수 없습니다.

#### 4. React에서의 사용 맥락
- 기능적으로는 두 방식 모두 React 함수형 컴포넌트로 동일하게 작동합니다.
- 현대 React 개발에서는 화살표 함수 스타일이 더 많이 사용되는 추세입니다.
- 클래스 컴포넌트와 달리 함수형 컴포넌트는 생명주기 메서드를 직접 사용하지 않습니다.

- 두 방식 간의 성능 차이는 무시할 수 있을 정도로 미미합니다.

대부분의 현대 React 코드베이스에서는 일관성을 위해 한 가지 스타일을 선택하여 사용합니다. 프로젝트나 팀의 코딩 컨벤션에 따라 선호되는 방식이 다를 수 있습니다.