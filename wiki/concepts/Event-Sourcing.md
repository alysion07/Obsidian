---
title: Event Sourcing Pattern
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 디자인패턴
  - 아키텍처
  - 이벤트
---

# Event Sourcing Pattern

Event Sourcing은 시스템의 현재 State를 직접 저장하는 대신, **State를 변경시킨 Event의 전체 이력을 저장**하는 아키텍처 패턴이다. 시스템의 State는 저장된 모든 이벤트들을 순차적으로 재생하여 계산한다.

즉, *'지금 상태가 뭐야?'* 가 아니라 *'어떤 일이 있었는지를 저장'* 하는 방식이다.

## 핵심 개념

- **Event**: State를 변경시킨 사실 (예: `UserCreated`, `ItemAddedToCart`, `OrderPaid`)
- **Event Store**: Event들을 시간 순으로 저장하는 로그
- **Event Handler/Projector**: 이벤트를 해석해서 상태를 갱신하는 컴포넌트
- **Rehydration(재생)**: 저장된 Event들을 차례대로 적용해 현재의 State를 복원

## JavaScript 활용 예시

```js
// 1. Event Log
const events = [
  { type: 'ITEM_ADDED', payload: { id: 1, name: 'Keyboard', price: 50 } },
  { type: 'ITEM_ADDED', payload: { id: 2, name: 'Mouse', price: 30 } },
  { type: 'ITEM_REMOVED', payload: { id: 1 } },
];

// 2. Rehydration: 'state' through 'events'
function replay(events) {
  const state = { cart: [] };

  for (const event of events) {
    switch (event.type) {
      case 'ITEM_ADDED':
        state.cart.push(event.payload);
        break;
      case 'ITEM_REMOVED':
        state.cart = state.cart.filter(item => item.id !== event.payload.id);
        break;
    }
  }
  return state;
}

console.log(replay(events));
// Output: { cart: [{ id: 2, name: 'Mouse', price: 30 }] }
```

이벤트 로그를 순차적으로 재생(replay)하여 현재 상태를 복원하는 방식으로, 상태 변경의 전체 이력을 추적할 수 있다는 장점이 있다.
