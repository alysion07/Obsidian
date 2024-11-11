#### 1. WebGLRenderingContext 함수에 `viewportWidth`와 `viewportHeight` 값을 넣는 행위 

```js 
gl = canvas.getContext("webgl2");
gl.viewportWidth = canvas.width;    // BAD!!!
gl.viewportHeight = canvas.height;  // BAD!!!
```

`canvas`의 크기를 변경할 때 마다 업데이트 해야하는 두 개의 속성이 생기기 때문. 사용자가 window의 크기를 조절할 때 캔버스의 크기를 변경하면 `gl.viewportWidth`와 `gl.viewportHeight`는 다시 설정하지 않는 한 잘못된 값을 가지게 됨.

```c
gl.viewport(0, 0, gl.canvas.width, gl.canvas.height);
```

컨텍스트 자체도 직접적으로 `width`와 `height`를 가지고 있음.

```c
// 캔버스의 drawingBuffer 크기에 맞춰 뷰포트를 설정해야 할 때
// 이 방법은 항상 정확합니다.
gl.viewport(0, 0, gl.drawingBufferWidth, gl.drawingBufferHeight);
```

더 좋은 점은, **`gl.drawingBufferWidth`** 와 **`gl.drawingBufferHeight`** 를 사용하면 극단적인 경우도 처리할 수 있는 반면, **`gl.canvas.width`** 와 **`gl.canvas.height`** 를 사용하는 경우에는 그렇지 않다는 것입니다. 그 이유는 [여기](https://webgl2fundamentals.org/webgl/lessons/webgl-anti-patterns.html#drawingbuffer)를 참고하세요.

#### 2. Aspect Ratio에 `canvas.width`, `canvas.height` 를 사용하는것. 

종종 `canvas.width`, `canvas.height`를 사용하여  종횡비(Aspect)를 구하는 경우가 있다. 
```js title:"Bad Case"
var aspect = canvas.width / canvas.height;
perspective(fieldOfView, aspect, zNear, zFar);
```

`canvas`의 `width`와 `height`는 `canvas`가 출력되는 크기와  아무 상관이 없다. **CSS**가 `canvas`가 보여지는 사이즈를 결정한다.

#### What to do Instead?
 **`canvas.clientWidth`** 와 **`canvas.clientHeight`** 를 사용하라. 이 두 값은 실제로 캔버스가 스크린에 표시되는 사이즈를 알려준다. 해당 값을 사용하면 CSS 설정에 상관없이 항상 정확한 종횡비를 얻을 수 있다.
 
 ```js hl:1 title:"CSS 설정과 관계없이 올바른 Aspect를 구하는 방법"
 var aspect = canvas.clientWidth / canvas.clientHeight;
perspective(projectionMatrix, fieldOfView, aspect, zNear, zFar);
```

