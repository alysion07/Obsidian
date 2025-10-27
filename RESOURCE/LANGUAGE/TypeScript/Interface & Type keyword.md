# Type Keyword

- `C++`의 `typedef` keyword와 유사함 
- Alias 생성 가능. 지정된 옵션으로만 제한 가능 


```ts title:"type keyword 활용"
// opt1 
type newNum = number
// opt2
type Team = "red" | "yellow" | "green"
type Health = 1 | 5 | 10
// opt3
type Player = {
	nickname: string
	team:Team
	health: Health
}


const nico :Player = {
	nickname:"nico"
	team: "yellow"
	health: 10 // if 9 error
}

// 여러 타입을 하나로 결합하는 역할을 합니다.
type PlayerAA = PlayerA & {
	lastName:string
}
```

# Interface
`object`, `class`의 모양을 설명할 때 사용한다.  **TypeScript**에서만 사용 가능한 키워드
구현해야 하는 `method`와 `property`를 강제할 수 있다. `abstract` `class`를 대체 사용 가능하며,  `implement` 키워드로 `interface`를 상속 가능함.


> **`interface`로 상속 받을 시, `property`에 `private`를 사용할 수 없음**
> 	`public`만 사용 가능하다는 단점이 있음

```ts 
interface User {
	name: string
}
interface User {
	lastName: string
}
interface User {
	health : string
}
izznterface Player extends  User {
}
const nico = Player = {
	name:"nico"  // 개별선언된 인스턴스를 알아서 하나로 합쳐줌
	lastName:"las"
	helath 2
}
```
  


## interface와 type의 차이


```ts title:"다양한 object 형태 정의 방법"
type PlayerA = {
	firstName:string
}
//invalid 
//type PlayerA = { 
	//middleName:string 
//}

interface PlayerB { 
	firstName:string
}
// valid
interface PlaterB { 
	middleName:string
}

class User implements PlayerB {
	constructor(	
		public firstName:string,
		public middleName:string
	){}
}
```

`object`,`class` 의 모양을 정의할 때에는 `interface`를 쓰고, 그 외 나머지 모든 경우는 `type`을 사용하는 것을 권장