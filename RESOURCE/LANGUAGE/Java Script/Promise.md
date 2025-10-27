## Overview

**Promise**는 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
callback을 전달하는 대신 callback을 첨부하는 방식의 객체

```js
function successCallback(result) {
  console.log("Audio file ready at URL: " + result);
}

function failureCallback(error) {
  console.log("Error generating audio file: " + error);
}

createAudioFileAsync(audioSettings, successCallback, failureCallback);
```

```js title:"Modern style"
createAudioFileAsync(audioSettings).then(successCallback, failureCallback);

// more simply 
const promise = createAudioFileAsync(audioSettings);
promise.then(successCallback, failureCallback);
```

## Guarantees

callback 함수를 전달하는 고전 방식과는 달리 **Promise** 는 아래와 같은 특징을 보장
 - callback은 JavaScript Event Loop가 *현재 실행 중인 콜 스택* 을 완료하기 이전에는 절대 호출되지 않는다.
 - 비동기 작업이 성공하거나 실패한 뒤에 `then()`을 이용하여 추가한 callback의 경우도 위와 같다.
 - `then()`을 여러번 사용하여 여러개의 callback을 추가할 수 있다. 각각의 callback은 주어진 순서대로 실행된다.

Promise의 가장 뛰어난 장점은 Chaining이다.

## Chaning


```js 
doSomething()
  .then((result) => doSomethingElse(result))
  .then((newResult) => doThirdThing(newResult))
  .then((finalResult) => {
    console.log(`Got the final result: ${finalResult}`);
  })
  .catch(failureCallback);
```

**중요:** 반환값이 반드시 있어야 합니다, 만약 없다면 callback 함수가 이전의 promise의 결과를 받지 못합니다. (화살표 함수 **`() => x`** 는 **`() => {return x;}`** 와 같습니다.)