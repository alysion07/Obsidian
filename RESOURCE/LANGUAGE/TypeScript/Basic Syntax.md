# Explicit Type

```ts
type Player = {      //  'Alias' 타입 생성 
	name : string,   // ': type': 변수 타입 지정 가능
	age?: number     // '?': 선택적 사용 가능
}

const nico : Player = {
	name :"nico",
}
const lynn : Player = {
	name : "lynn",
	age : 12,
}

// `arrow function` type setting 
const playerMaker  (name:string) : Player => ({name})
// 'argument type' and 'return type' setting 
function platerMaker( name : string, age?: number ) : Player {
	return {
		name
	}
}
```

- `: type` : 세미콜론 뒤 타입을 설정하여 변수 선언 가능 
- `?`: 변수 선택적  사용 가능>>

# Syntax only available in 'TypeScript

```ts title:"Syntax only available in 'TypeScript'" er:3,7,19
// Tuple Usage
const player = readonly [string, number, boolean] = ["nico", 1, true]
player[0] = "hi" // error, 0 index is readonly

// unknown type
let a: unknown;
let b = a + 1; // invalid. 'a' is unknown type
if(typeof a === 'number'){
	let b = a + 1 // valid
} else if (typeof a === 'number') { // type check
	let b = a.toUpperCase(); // valid 
}
// 'void' usage
function hello() {
 console.log("hello")
 // return 
}
const c = hello();
c.toUpperCase(); // invalid. hello is void function
```


```ts title:"'never' keyword"
function hello(name: string|number) {
	if (typeof name === "string"){
		name // name type is string
	}
	else if (typeof name === "number") {
		name // name is number
	} else {
		name  // name is never type
	}
}

function bye() : never {
	throw new Error("xxx");
	// return is invalid 
}


```

반환형이 `never` 인 경우, 혹은 타입이 두가지 일 수도 있는 상황에 사용


# Call signatures

```ts 

// basic synrax 
function or_Add(a: number, b: number ) {  
    return a + b + 1;  
}

// Call Signatures  
type Add = (a: number, b: number) => number;  
  
// 'add'함수에 'Add' Call Signature를 설정해줌으로써 
// 파라미터 a,b의 타입 작성필X
const add:Add = (a, b) => a + b; 
```


# Overloading 
```ts
type Add = {
	(a: number, b:number)  : number
	(a: number, b:number, c:number) : number,
}
// 'c?' 없을 경우 처리
const fadd: functionAdd = (a, b, c?: number) => {  
    if (c) return a + b + c; // exist 'c'
    return a + b;            // not exist 'c'
}
```

# Geniric 

``` ts
type SpuperPrint = { 
	<T> ( arr: T[] ) : T
 } 

const superPrint: SpuperPrint = (arr) => arr[0]  
superPrint([1, 2, 3, 4, 5]);        //T -> number
superPrint([false, false, true]);   //T -> boolean
superPrint(["a", "b", "c"]);        //T -> string
superPrint([1, 2, true, false, "hello", "world"]); //T -> n, b, s
```


# class
 [[Class]] 별도 정리 
