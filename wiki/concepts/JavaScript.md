---
title: JavaScript
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - JavaScript
  - 프로그래밍
  - 웹개발
---

# JavaScript

## 개요

JavaScript는 웹 브라우저에서 동작하는 프로그래밍 언어로, 현대 웹 개발의 핵심 언어이다. Single Threaded 기반으로 동작하며, 비동기 처리를 위한 다양한 메커니즘을 제공한다.

## Single Threaded Language

JavaScript는 하나의 Call Stack만을 가진 싱글 스레드 언어이다. 처리가 오래 걸리는 코드들은 별도의 대기실(Web API)로 보내 처리한다.

```js
console.log(1+1)        // 2 (즉시 실행)
setTimeout(function(){ console.log(2+2) })  // 4 (대기 후 실행)
console.log(3+3)        // 6 (즉시 실행)

// 출력 순서: 2, 6, 4
```

### 대기실로 보내지는 코드들

- **Ajax** 요청 코드
- `EventListener`
- `setTimeout`

이러한 코드들은 Web API로 전달되어 처리된 후, Callback Queue를 거쳐 Event Loop에 의해 다시 Call Stack으로 돌아온다.

## Event Loop

Event Loop는 Call Stack이 비어있을 때 Callback Queue에서 대기 중인 작업을 꺼내 Call Stack에 넣어주는 역할을 한다. 이 메커니즘 덕분에 싱글 스레드임에도 비동기 처리가 가능하다.

## Promise

**Promise**는 비동기 작업의 최종 완료 또는 실패를 나타내는 객체이다. Callback을 전달하는 대신 callback을 첨부하는 방식으로 동작한다.

```js
// 전통적 callback 방식
createAudioFileAsync(audioSettings, successCallback, failureCallback);

// Promise 방식
createAudioFileAsync(audioSettings).then(successCallback, failureCallback);
```

### Promise의 보장 사항

- Callback은 Event Loop가 현재 실행 중인 콜 스택을 완료하기 이전에는 절대 호출되지 않는다.
- 비동기 작업 완료 후 `then()`으로 추가한 callback도 동일하게 동작한다.
- `then()`을 여러 번 사용하여 여러 개의 callback을 순서대로 실행할 수 있다.

### Chaining

Promise의 가장 뛰어난 장점은 Chaining이다.

```js
doSomething()
  .then((result) => doSomethingElse(result))
  .then((newResult) => doThirdThing(newResult))
  .then((finalResult) => {
    console.log(`Got the final result: ${finalResult}`);
  })
  .catch(failureCallback);
```

> **중요:** 반환값이 반드시 있어야 한다. 없으면 다음 callback이 이전 Promise의 결과를 받지 못한다. 화살표 함수 `() => x`는 `() => { return x; }`와 같다.

## async / await

`async`와 `await`는 Promise를 더 간결하게 사용할 수 있게 해주는 ES2017 문법이다. `async` 함수는 항상 Promise를 반환하며, `await`는 Promise가 처리될 때까지 함수 실행을 일시 정지한다.

## 클로저 (Closure)

클로저는 함수와 그 함수가 선언된 Lexical Environment의 조합이다. 내부 함수가 외부 함수의 변수에 접근할 수 있는 메커니즘으로, 실행 컨텍스트와 활성 객체(Activation Object)를 통해 동작한다. JavaScript의 스코프 체인과 밀접한 관련이 있다.

## 주요 문법 요소

- **Arrow Function:** `() => {}` 형태의 함수 표현식. 자신만의 `this`를 생성하지 않고 렉시컬 스코프의 `this`를 사용한다.
- **ASI (Automatic Semicolon Insertion):** 자동 세미콜론 삽입 기능
- **비트 연산:** 비트 단위 연산자 지원
- **reduce:** 배열 메서드로, 누적값을 활용한 배열 처리

## 관련 문서

- [[TypeScript]] - JavaScript의 정적 타입 슈퍼셋
- [[React]] - JavaScript 기반 UI 라이브러리
- [[Three-js]] - JavaScript 3D 라이브러리
- [[Networks]] - 웹 통신의 기반 네트워크 개념
