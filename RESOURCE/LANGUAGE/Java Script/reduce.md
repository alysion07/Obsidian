
## reduce 함수란?

reduce는 사전적을 줄이다라는 뜻.
배열을 순차적으로 순회하면서 **하나의 값으로 줄여 _return_** 하는 함수이다.
즉 배열을 기반으로 하나의 값을 도출할 때 사용된다.

## 기본 문법

```js
arr.reduce(callback(accumulator, currentValue, index, array), initialValue);
// 배열.reduce(callback(누적값, 현재값, 인덱스, 요소), 초기값);
```



### example

```js  title:'배열 요소의 합산'
const arr = [1, 2, 3, 4, 5]
  
arr.reduce(function (acc, cur, idx) {
  console.log(acc, cur, idx);
  return acc + cur;
}, 0);
 
// arr cur idx
// 0 1 0
// 1 2 1
// 3 3 2
// 6 4 3
// 10 5 4
```

코드와 출력 결과를 살펴보자 초기값(0)에 누적값(arr)에 배열의 요소(cur)를 더한 값을 return 하도록 하는 code다.

초기값을 0 주었으므로 첫번째 순번에는 0 + 1을 return 한다. 그리고 출력값을 보면 return 된 값은 누적값(acc)로 할당되는 걸 확인할 수 있다. 즉, 더해진 return 값은 순차적으로 누적값(acc)로 전달된다.

_만약 초기값을 정해주지 않았다면 배열의 0번 index부터 시작한다._

```js title:'map 구현'
const arr = [1, 2, 3, 4, 5]
  
const result = arr.reduce(function (acc, cur) {
  acc.push(cur % 2 ? "홀수" : "짝수");
  return acc;
}, []);
 
console.log(result);
// [ '홀수', '짝수', '홀수', '짝수', '홀수' ]
```

map함수는 기존 배열과 다른 새 배열(다른 객체)을 리턴한다. reduce도 초기값을 빈 배열로 제공해주고, push와 같은 배열 메서드를 사용하면 map처럼 새로운 배열을  반환할 수 있다


#### **💡 이 밖의 reduce 활용 예**

- object 그룹핑 / 카운팅
- 배열 flatten
- filter & map
- data type의 변환
- 비동기 프로그래밍
---
## ETC

- 일반 함수로 작성하여 reduce 예시를 들었지만, 화살표 함수를 사용하면 좀 더 간결한 코드를 구성할 수 있다.
- 만약 배열을 반대방향으로부터 탐색하고 싶다면. 맨 뒤 index로부터 순회하는 reduceRight를 사용하자.
- reduce만 잘 알고 있으면 다른 배열 메서드들을 구현할 수 있다. 문제를 해결하다가 다른 메서드를 잊어버렸다면 reduce로 구현해볼 수 있다. (sort, filter, every, some, find, findIndex, includes 등)
- reduce는 이와같이 활용도가 아주 높고 강력한 메서드이므로 꼭 기억해두면, 코드의 성능을 높이고 간소화 할 수 있을 것이다.