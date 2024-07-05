모든 캔버스는 2개의 크기를 가짐 

1. drawingbuffer
2. canvas의 diplay szie(화면에 표시되는 크기. CSS가 화면에 표시되는 크기를 결정함)

```html
<canvas id="c" width = "400" height="300"></canvas>
```

다른 하나는 JavaScript를 사용하는 것

```html
<canvas id="c"></canvas>
```

```js
const canvas = ducument.querySelector('#c');
canvas.width = 400;
canas.height = 300;
```

> [!NOTE]
> canvas의 크기는 CSS가 없다면 **drawingbuffer**의  사이즈로 출력됨. 



## 예시 1. drawingbuffer 10X15 / DisplaySize 400X300
```html
<canvas id="c" width="10" height="15" style="width: 400px; height: 300px;"></canvas>
```

same example
```html
<style>
#c {
	width: 400px;
	height: 300px;
}
</style>
<canvas id="c" width="10" height="15"></canvas>
```

> 실행 결과: 15 사이즈가 300px로 확대되어 보임

---
## 개선 방안

### `clientWidth` & `clientHeight` 사용

가장 쉬운 방법. `clientWidth`와 `clientHeight`는 HTML의 모든 요소(element)가 가진 속성
CSS pixel 단위로 요소의 크기를 알려줌.

> [!NOTE] Warnning
> client 사각형은 CSS 패딩을 포함하므로 `clientWidth`와 `clientHeight`를 사용할 때는 캔버스 요소에 패딩을 사용하지 않는 것이 좋습니다.


JavaScript를 사용하여 요소가 화면에 보여지는 크기를 확인하고, drawingBuffer의 크기를 그 값에 맞춰준다.

```js
function resizeCanvasToDisplaySize(canvas) {
// 브라우저가 캔버스를 표시하고 있는 크기를 CSS pixel 단위로 얻어옴
const displayWidth = canvas.clientWidth;
const displayHeight = canvas.clientHeight;

//캔버스와 크기가 다른지 확인한다.
const needResize = canvas.width !== displayWidth || canvas.height !== displayHeight;

if(needResize) {
// 캔버스를 동일한 크기가 되도록 설정
	canvas.width = displayWidth;
	canvas.height = displayHeight;
}
return needResiz;
}
```

위 함수를 렌더링을 수행하기전에 호출해서 항상 원하는 크기가 되도록 설정

```js 
function drawScene() {
	resizeCanvasToDisplaySize(gl.canvas);
	// ... do something
}
```

> 결과:  1차 개선만으로는 화면에 선이 꽉 채워 지지않음. 캔버스는 커짐


이유는 캔버스의 사이즈를 수정하기 전, `gl.viewport`를 호출하여 뷰포트를 설정해야 하기  때문.

`gl.viewport` : WebGL에게 클립 공간(-1 ~ 1)에서 픽셀로 변환하는 법과 그러한 변환이 캔버스의 어떤 부분에서 이뤄져야 하는지 알려줌. context 생성시, WebGL은 뷰포트를 캔버스의 크기와 동일 하도록 설정해줌. 하지만 그 이후엔 사용자가 설정해야 함. 만일 캔버스의 크기를 바꿨다면, WebGL에게 viewport 설정을 알려줘야 한다.

```js hl:4
function drawScene() {
	resizeCanvasToDisplaySize(gl.canvas);
	
	gl.viewport(0,0, gl.canvas.width, gl.canvas.height);
}
```


---


## `devicePixelRatio`와 Zoom 다루기


사용자가 **HD-DPI** 디스플레이를 사용하는지, 아니면 **확대(zoom in)** 또는 **축소(zoom out)** 했는지, 아니면 OS의 확대 수준을 설정 했는지에 따라 실제로 모니터에 표시되는 픽셀의 숫자는 달라질 수 있음.


``` js hl=4,5
function resizeCanvasToDisplaySize(canvas) {
// 브라우저가 캔버스를 표시하고 있는 크기를 CSS 픽셀 단위로 얻어옵니다.

//const displayWidth = canvas.clientWidth;
//const displayHeight = canvas.clientHeight;
const dpr = window.devicePixelRatio;
// devicePixelRatio는 정수가 아니기 때문에 Math.round()로 보정
const displayWidth = Math.round(canvas.clientWidth * dpr);
const displayHeight = Math.round(canvas.clientHeight * dpr);

// 캔버스와 크기가 다른지 확인합니다.
const needResize = canvas.width != displayWidth ||
canvas.height != displayHeight;

if (needResize) {
// 캔버스를 동일한 크기가 되도록 합니다.
canvas.width = displayWidth;
canvas.height = displayHeight;
}

return needResize;
}
```


---

##  `getBoundingClientRect` 사용

`width` 와 `height`를 가진 `DOMRect`를 반환
`clientWidth`와 `clientHeight`로 표현되는 사각형이지만 정수형을 요구하지 않음

```js hl:7,8 ar:6 er:4,5
function resizeCanvasToDisplaySize(canvas) {
  // 브라우저가 캔버스를 표시하고 있는 크기를 CSS 픽셀 단위로 얻어옵니다.
  const dpr = window.devicePixelRatio;
 // const displayWidth  = Math.round(canvas.clientWidth * dpr);
//  const displayHeight = Math.round(canvas.clientHeight * dpr);
  const {width, height} = canvas.getBoundingClientRect();
  const displayWidth  = Math.round(width * dpr);
  const displayHeight = Math.round(height * dpr);
 
  // 캔버스와 크기가 다른지 확인합니다.
  const needResize = canvas.width  != displayWidth || 
                     canvas.height != displayHeight;
 
  if (needResize) {
    // 캔버스를 동일한 크기가 되도록 합니다.
    canvas.width  = displayWidth;
    canvas.height = displayHeight;
  }
 
  return needResize;
}
```

 **그러나!**  `canvas.getBoundingClientRect()`는 언제나 올바른  값을 반환하는 것이 아님
 HTML 수준을 넘어서서 컴포지터(compositer) 수준도 영향을 미침
 
 예를 들어 999pixel의 window가 있고, devicePixelRatio가 2.0인 상황이라고 가정하고,
 두 개의 width: 50%인 요소를 나란히 만들었다고 하면 HTML에서는 499.5로 계산되지만 실제로 컴포지터가 실제로 그릴 때에는 499 / 500 pixel로 그려짐.

브라우저 벤더들이 찾은 해결책은 [`ResizeObserver`](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver) API를 사용하여, 실제 사용되는 크기를 `devicePixelContextBoxSize` 속성을 통해 제공하는 것이었음.

`devicePixelContextBoxSize` : 실제 사용된 장치 Pixel을 반환.
`ClientBox`가 아닌 `ContentBox`라 이름 붙여진 것에 주의.
이는 캔버스 요소에서 _컨텐츠_ 를 보여주는 부분을 의미한다는 뜻이고, 
따라서 `clientWidth`, `clientHeight`와 `getBoundingClientRect`처럼 패딩을 포함하지 않음

- `ResizeObserver`는 모든 모던 브라우저에서 제공
- 하지만 `devicePixelContentBoxSize`는 Chrome/Edge 브라우저에서만 사용 가능

```js
const resizeObserver = new ResizeObserver(onResize);
resizeObserver.observe(canvas, {box: 'content-box'});
```

캔버스의 크기가 윈도우의 크기의 100%인 경우,
캔버스가 확대/축소 수준에 상관없이 항상 같은 디바이스 픽셀을 가짐. 반면에 `content-box`는 확대/축소를 하면 변하게 됨. CSS 픽셀 단위로 측정되기 때문

```js
const resizeObserver = new ResizeObserver(onResize);
try {
// 디바이스 픽셀이 변할 경우에만 호출
resizeObserver.observe(canvas, {box: 'device-pixel-content-box'});
} catch (ex) {
// device-pixel-content-box를 지원하지 않는 경우에는 아래 코드를 사용
resizeObserver.observe(canvas, {box: 'content-box'});
}
```

`onResize`함수는 [`ResizeObserverEntry`s](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserverEntry) 배열에 의해 호출됩니다. 크기가 변한 요소마다 한 번씩 호출됩니다. 하나 이상의 요소를 처리할 수 있도록 크기를 map에 기록해 둘 것입니다.

``` js
const canvasToDisplaySizeMap = new Map([[canvas, [300, 150]]]);

function onResize(entries) {
	for (const entry of entries) {
		let width;
		let height;
		let dpr = window.devicePixelRatio;
		if(entry.devicePixcelContentBoxSize) {
			// 주의: 이 경우에만 올바른 결과가 도출됨
			// 다른 경우, 불완전한 처리가 수행되는데, 
			// 브라우저가 이러한 문제를 해결하는 방법을 제공하지 않는 경우임
			width = entry.devicePixelContentBoxSize[0].inlineSize;
			height = entry.devicePixelContentBoxSize[0].blockSize;
			dpr = 1; // 이미 width, height 값에 포함됨			
		} 
		else if (entry.contentBoxSize) {
			if(entry.contentBoxSize[0]) {
				width = entry.contentBoxSize[0].inlineSize;
				height = entry.contentBoxSize[0].blockSize;
			} else {
				width = entry.contentBoxSize.inlineSize;
				height = entry.contentBoxSize.blockSize;
			}
		} else {
			width = entry.contentRect.width;
			height = entry.contentRect.height;
		}
	const displayWidth = Math.round(width * dpr);
	const displayHeight = Math.round(height * dpr);
	canvasToDisplaySizeMap.set(entry.target, [displayWidth, displayHeight]);
	}
}
```

---
## 코드 개선

```js hl:8 er:3-6
function resizeCanvasToDisplaySize(canvas) {
// Lookup the size the browser is displaying the canvas in CSS pixels.
//const dpr = window.devicePixelRatio;
//const {width, height} = canvas.getBoundingClientRect();
//const displayWidth = Math.round(width * dpr);
//const displayHeight = Math.round(height * dpr);
// 디바이스 픽셀 단위로 브라우저가 이 캔버스를 표시하는 크기를 얻어옵니다.
const [displayWidth, displayHeight] = canvasToDisplaySizeMap.get(canvas);
 
  // 캔버스와 크기가 다른지 확인합니다.
const needResize = canvas.width  != displayWidth || 
				   canvas.height != displayHeight;
 
if (needResize) {
    // 캔버스를 동일한 크기가 되도록 합니다.
	canvas.width  = displayWidth;
	canvas.height = displayHeight;
}
 
  return needResize;
}

```

나아가서, `devicePixelRatio`를 무작정 사용하는 것은 성능 저하를 불러일으킬 수 있습니다. iPhoneX나 iPhone11에서는 `window.devicePixelRatio`가 `3`인데 이는 9배나 많은 픽셀을 그리게 된다는 뜻입니다. Samsung Galaxy S8에서는 그 값이 `4`인데 이는 16배 많은 픽셀을 그린다는 뜻입니다. 이는 여러분 프로그램을 느리게 만듭니다.

어떤 방법을 사용할 것인지는 여러분에게 달렸습니다. 저같은 경우 99%의 경우 `devicePixelRatio`를 사용하지 않습니다. 이는 대부분의 사람들은 눈치채지 못하는 몇 가지 그래픽적인 장점만을 제공하면서 페이지를 느리게 만들기 때문입니다. 이 사이트에서 몇 개의 다이어그램에는 이 기능을 사용하지만 대부분의 예제에서는 사용하지 않고 있습니다.

여러 WebGL 프로그램에서 리사이징이나 캔버스의 크기를 셋팅하는 서로 다른 방법들을 보실 수 있을겁니다. 제 생각에는 브라우저가 디스플레이되는 캔버스의 크기를 CSS를 통해 결정하고 그 뒤에 결정된 크기를 기반으로 캔버스에 얼마나 많은 필셀을 그릴지를 조정하는 것이 가장 좋은 방법인 것 같습니다. 이유가 궁금하시다면 [여기 몇 가지 이유가 있습니다.](https://webgl2fundamentals.org/webgl/lessons/ko/webgl-anti-patterns.html) 위에 말씀드린 방법이 가장 선호되는 방법일 것 같습니다.