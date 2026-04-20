
```ts 
type Words = {  
    [key: string]: string;  
}  
  
class Dict {  
    private words: Words;  
  
    constructor() {  
        this.words = {};  
    }  
  
    add(word: Word) {  
        if (this.words[word.term] === undefined) {  
            this.words[word.term] = word.def  
        }  
    }  
  
    def(term: string) {  
        return this.words[term];  
    }  
}  
  
class Word {  
    constructor(  
        public term: string,  
        public def: string,  
    ) {  
    };  
}  
  
const kimchi = new Word("kimchi", "한국의음식")  
const dict = new Dict();  
  
dict.add(kimchi)  

dict.def("kimchi") // print "한국의음식"
```


# Abstract 

#abstract 

Abstract Class: 추상 클래스
Abstract Function: Call Signature만 선언해 놓은 형태 ->  **순수 가상 함수**

:RiJavascriptFill:코드로 변환시 불필요한 클래스가 생성되어 버림. 코드 간소화를 위해 TypeScript에서는 `Interface`를 활용한 형태로 더 자주 쓰임

```ts 
abstract class User {
	constructor( 
		protected firstName:string,
		protected lastName:string
	) {}
	abstract sayHi(name:string): string,
	abstract fullName():string,
}

class Player extends User {
	fullname() {
		return `${firstName}`
	}
}
```

```ts title:"interface를 활용한 추상클래스 제작"
interface User {
    firstName: string,
    lastName: string,

    sayHi(name: string): string

    fullName(): string
}
interface Human {
    health: number
}

class Player implements User, Human { // 다중 상속 가능
    constructor(
        public firstName:string,
        public lastName:string,
        public health:number
    ) {}


    sayHi(name: string): string {
        return `Hello ${name} My name is ${this.fullName()}`;
    }

    fullName(): string {
        return `${this.firstName} ${this.lastName}`
    }

}
```


# Polymorphism

```ts
interface SStorage<T> {
    [key:string]: T
}

class LocalStorage<T> {
    private  storage: SStorage<T> = {}
    set(key:string, value:T){
        this.storage[key] = value;
    }
    remove(key:string){
        delete this.storage[key];
    }
    get(key:string):T {
        return this.storage[key];
    }
    clear() {
        this.storage = {};
    }
}

const stringStorage = new LocalStorage<string>();

stringStorage.get("name");
stringStorage.set("hello", "sdasd");

const booleanStorage = new LocalStorage<boolean>();

booleanStorage.set("isCool", true);
booleanStorage.set("hello", false);
```