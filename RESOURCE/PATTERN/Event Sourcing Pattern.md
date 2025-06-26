Event Sourcing은 시스템의 현재 State를 직접 저장하는 대신, **State를 변경시킨 Event를 의 전체 이력을 저장**하는 아키텍쳐 패턴이다. 
- 시스템의 State는 저장된 모든 이벤트들을 순차적으로 재생하여 계산 한다. 
- 즉, *'지금 상태가 뭐야?'* 가 아니라 *'어떤일이 있었는지를 저장'*하는 방식

### 핵심 개념
- **Event**: State를 변경시킨 사실(예: `UserCreated`, `ItemAddedToCart`, `OrderPaid`)
- **Event Store**: Event들을 시간 순으로 저장하는 로그 
- **Evnet Handler/Proejctor**: 이벤트를 해석해서 상태를 갱신하는 컴포넌트
- **Rehydration(재생)**: 저장된 Event들을 차례대로 적용해 현재의 State를 복원
### ✏️JavaScrtipt에서의 활용

``` js
// 1. Event Log
const event = [
	{ type: 'ITEM_ADDED', payload: { id: 1, name: 'Keyboard', price: 50 } },
	{ type: 'ITEM_ADDED', payload: { id: 2, name: 'Mouse', price: 30 } },
	{ tpye: 'ITEM_REMOVED', payload: { id: 1 } },
]

// 2. Rehydration 'state' through 'event'
function replay(event) {
	const state = { cart: [] };

	for ( const event of events ) {
		switch (event.type) {
			case 'ITEM_ADDED':
				state.cart.push(event.payload);
				break;
			case 'ITEM_REMOVED':
			    state.cart.filter(item => item.id !== event.payload.id);
				break;
		}
	}
	return state;
}
console.log(replay(events));
// Output: { cart: [{ id: 2, name: 'Mouse', price: 30 }] }
```

