## document 

하나의 큰 객체 HTML이 `.js` 코드를 **load**하기 때문에 존재하는 객체. JavaScript는 **document object**를 통해 HTML과 **소통**할 수 있다.

```js
// title 접근
document.title = "Hi From JS";
// get location
document.location;
// same command
document.querySelector("#hello h1");
document.getElementById("hello");
```

### Element를 가져오는 방법

#### querySelector 

**CSS Selector**를 HTML로 전달하여, JavaScript로 Element를 가져올 수 있음.


---

### Element를 추가하는 방법

#### createElement(**tagname**: string, **option?** ElementCreationOptions )

HTML 문서에서, **`Document.createElement()`** 메서드는 지정한 `tagName`의 HTML 요소를 만들어 반환한다. `tagName`을 인식할 수 없으면 [`HTMLUnknownElement`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLUnknownElement)를 대신 반환함.


---
## Events
###### addEventListener("*evt_name*", *handleFunction*)

``` js
window.addEventListener("offline", hendleWindowOnline);
```

###### event.preventDefault()

해당 이벤트의 기본 동작을 막는다.  
	ex. 링크가 걸린 텍스트 클릭 -> 링크 이동 제한

