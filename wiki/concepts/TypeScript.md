---
title: TypeScript
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - TypeScript
  - 프로그래밍
  - 타입시스템
---

# TypeScript

## 개요

TypeScript는 JavaScript의 정적 타입 슈퍼셋으로, 컴파일 타임에 타입 검사를 수행하여 런타임 에러를 사전에 방지한다. JavaScript에서 사용할 수 없는 고유 문법(Tuple, unknown, never 등)을 제공한다.

## Explicit Type (명시적 타입)

변수, 함수 매개변수, 반환값에 타입을 명시할 수 있다.

```ts
type Player = {
    name: string,
    age?: number     // '?': 선택적 사용 가능
}

// Arrow function 타입 설정
const playerMaker = (name: string): Player => ({ name })

// 일반 함수 타입 설정
function playerMaker(name: string, age?: number): Player {
    return { name }
}
```

- `: type` -- 세미콜론 뒤 타입을 설정하여 변수 선언
- `?` -- 선택적 프로퍼티 지정

## TypeScript 고유 문법

### Tuple

```ts
const player: readonly [string, number, boolean] = ["nico", 1, true]
```

### unknown 타입

```ts
let a: unknown;
let b = a + 1;  // 오류: 'a'가 unknown 타입
if (typeof a === 'number') {
    let b = a + 1  // 유효
}
```

### void와 never

- `void`: 반환값이 없는 함수의 반환 타입
- `never`: 절대 반환되지 않는 함수, 또는 타입 좁히기에서 도달 불가능한 분기에 사용

```ts
function bye(): never {
    throw new Error("xxx");
    // return은 불가능
}
```

## Call Signatures

함수의 타입을 별도로 정의하여 재사용할 수 있다.

```ts
type Add = (a: number, b: number) => number;

// Call Signature 적용 시 매개변수 타입 생략 가능
const add: Add = (a, b) => a + b;
```

## Overloading

하나의 함수에 여러 호출 시그니처를 정의한다.

```ts
type Add = {
    (a: number, b: number): number
    (a: number, b: number, c: number): number
}

const add: Add = (a, b, c?: number) => {
    if (c) return a + b + c;
    return a + b;
}
```

## Generic

타입을 매개변수처럼 사용하여 다양한 타입에 대응하는 재사용 가능한 코드를 작성한다.

```ts
type SuperPrint = {
    <T>(arr: T[]): T
}

const superPrint: SuperPrint = (arr) => arr[0]
superPrint([1, 2, 3, 4, 5]);          // T -> number
superPrint([false, false, true]);      // T -> boolean
superPrint(["a", "b", "c"]);          // T -> string
```

## Interface vs Type

### Type 키워드

- C++의 `typedef`와 유사하게 Alias 생성 가능
- 지정된 옵션으로만 제한 가능 (Discriminated Union)
- 여러 타입을 `&`로 결합 가능 (Intersection Type)

```ts
type Team = "red" | "yellow" | "green"
type Health = 1 | 5 | 10

type Player = {
    nickname: string
    team: Team
    health: Health
}
```

### Interface 키워드

- `object`, `class`의 모양을 설명할 때 사용
- TypeScript에서만 사용 가능한 키워드
- 구현해야 하는 method와 property를 강제할 수 있다
- `abstract class`를 대체하여 사용 가능하며, `implements` 키워드로 상속 가능
- 동일 이름으로 여러 번 선언하면 자동으로 병합된다 (Declaration Merging)

```ts
interface User {
    name: string
}
interface User {
    lastName: string  // 자동 병합
}
```

### 사용 가이드

- `object`, `class`의 모양을 정의할 때는 `interface` 사용
- 그 외 나머지 모든 경우는 `type` 사용 권장

> **주의:** `interface`로 상속받을 시 `property`에 `private`를 사용할 수 없고, `public`만 사용 가능하다는 제약이 있다.

## 관련 문서

- [[JavaScript]] - TypeScript의 기반 언어
- [[React]] - TypeScript와 함께 자주 사용되는 UI 라이브러리
