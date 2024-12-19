
자바스크립트는 브라우저뿐만아니라 서버에서도 실행할 수 있다. 이 외에도 [자바스크립트 엔진](https://en.wikipedia.org/wiki/JavaScript_engine)이라 불리는 특별한 프로그램이 있는 모든 디바이스에서 동작함



### 브라우저에서 할 수 있는 일
- 페이지에 새로운 HTML을 추가하거나 기존 HTML, 혹은 스타일 수정하기
- 마우스 클릭이나 포인터의 움직임, 키보드 키 눌림 등과 같은 사용자 행동에 반응하기
- 네트워크를 통해 원격 서버에 요청을 보내거나, 파일 다운로드, 업로드하기(AJAX나 COMET과 같은 기술 사용)
- 쿠키를 가져오거나 설정하기. 사용자에게 질문을 건네거나 메시지 보여주기
- 클라이언트 측에 데이터 저장하기(로컬 스토리지)
### 브라우저에서 할 수 없는 일
- 디스크에 저장된 임의의 파일을 **읽거나 쓰고** **복사**하거나 **실행**할 때 제약을 받을 수 있다. 운영 체제가 지원하는 기능을 **브라우저가 직접 쓰지 못하게 막혀있음** 모던 브라우저를 사용하면 파일을 다룰 순 있으나, 접근은 제한됨. 
  
  사용자가 브라우저 창에 파일을 끌어다 두거나,  `<input>` **태그를 통해 파일을 선택할 때와 같이 특정 상황**에서만 파일 접근을 허용 

-  브라우저 내 탭과 창은 대개 서로의 정보를 알 수 없다. 하지만 자바스크립트를 사용해 **한 창**에서 **다른 창을 열 때는 예외가 적용**. 하지만 이 경우에도 도메인이나 프로토콜, 포트가 다르다면 페이지에 접근할 수 없음. 이런 제약 사항을 **Same Oring Policy(동일 출처 정책)** 이라 부른다. 이 정책을 피하려면 *두 페이지*는 데이터 교환에 동의해야 한다. 
- 자바스크립트를 이용하면 페이지를 생성한 서버와 쉽게 정보를 주고받을 수 있다. 하지만 **타 사이트나 도메인에서 데이터를 받아오는 건 불가능**합니다. 가능하다 할지라도 원격 서버에서 **명확히 승인**을 해줘야 합니다(HTTP 헤더 등을 이용). 이 역시 보안을 위해 만들어진 제약사항



---

## Type 

### Number
 - Intiger: 정수형
 - flaot  :  실수형

### String (text)
- 문지열

### const / let 

const: 값을 업데이트할 수 없음. 기본적으로 모두 `const`를 사용하여 변수를 생성하고, 업데이트가 필요한 경우 `let`로 수정. 
`var` 구시대 유물임 사용 X

          
Boolean 
- tru / false

### null
null 값은 자연적으로 생겨나지 않음 변수안에 어떤 값이 없다는것을 알려주기 위해 사용함


### Arrays
초기화: ` const daysOfWeek = [ "mon", ]` 
추가 push(value) `array.push(v)`


Object 
init : `{}`

---
### 함수의 종류

선언식: `function` keyword를 선두로 함수를 작성

표현식: 변수에 할당하듯, 함수 이름을 변수에 할당하고 함수를 작성한다.

```js
let name = function(parameter) { ... return;  }
```


---

## HTML과의 연동




### document 
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


#### querySelector 
**CSS Selector**를 HTML로 전달하여, JavaScript로 Element를 가져올 수 있음.