하나의 Stack이 존재하며 
처리가 오래 걸리는 코드들은 대기실(?)로 보내 별도로 처리한다.
```js
console.log(1+1)
// 대기실 이동 대상 코드
setTimeout(function{console.log(2+2)})


console.log(3+3)


// output
2
6
4 // setTimeout result
```

#### 대기실로 보내지는 코드들 

- **Ajax** 요청 코드, `EventListener`, `setTimeout`


![[Pasted image 20250108102642.png]]