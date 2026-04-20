#화살표 #arrow

```js
let func = (arg1, arg2, ... argN) => expression
```

인자`arg1 ...argN`을 받는 함수 `func`가 선언된다. 함수 `func`는 화살표`(=>)` 우측의 `표현식(expression)` 을 평가하고 평가 결과를 반환한다. 즉, 아래 함수의 축약 버전

```js
let func = function(arg1, arg2, ...argN) {
	return expression;
}
```