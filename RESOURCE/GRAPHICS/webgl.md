--- 
[OpenGL ES](https://www.khronos.org/opengles/)

기초 강의: [WebGL Basic](https://webgl2fundamentals.org/webgl/lessons/ko/webgl-fundamentals.html)

---

WebGL2는 WebGL1과 거의 100% 역호환이 된다.

WebGL은 사용자의 GPU에서 실행됨. 즉 GPU에서 실행되는 코드를 제공되어야 하며, 그 코드는 두 개 함수 쌍 형태로 제공되어야함.
 각각의 함수는 정점 셰이더(vertex shader)와 프래그먼트 셰이더(fragment shader)로 불린다.
 [[Shader]]는 C/C++과 유사한 **GLSL(GL Shader Language)** 로 작성되어야 함. 
 이 한쌍을 합쳐서 **프로그램**이라고 부른다.


### Vertex Shader
---
정점 셰의더의 역할은 정점의 위치를 계산하는것. 함수를 통해 출력 위치가 계산되고, WebGL은 그 위치를 기준으로 'Point', 'Line', 'Triangle'을 비롯한 다양한 종류의 기본요소(Primitives)를 **래스터화** 할 수 있다. 

### Fragment Shader
---
이 기본요소들을 래스터화 할 때 두 번째로 제공한 Fragment Shader를 호출함.  Fragment Shader의 역할은 현재 그려지고 있는 기본요소의 각 픽셀 색상을 계산한다는 것.

거의 모든 WebGL API는 이러한 함수 쌍을 실행하기 위한 상태를 설정하기 위해 존재 함.
그리려는 요소마다 다양한 상태를 설정해 준 다음, GPU에서 Shader를 실행하는 `gl.drawArrays` 또는 `gl.drawElements`를 호출해서 함수 쌍을 실행한다.

### Buffer
---
버퍼는 GPU에 올라가는 Binary Data Array 임. 물론 어떤 데이터든 원하는대로 넣을 수 있음. 일반적으로는 Position, Normal, Texture Coordinate, Vertex Color 등등의 항목 포함.

### Attribute 
---
Attribute는 어떻게 버퍼에서 데이터를 가져올지, 그리고 Vertex Shader에 어떻게 전달할지 명시하기 위해 사용됨

각각의 Attribute가 사용할 버퍼는 무엇인지, 버퍼로부터 데이터를 추출하는 방법과 같은 Attributes 상태는 VAO(Vertex Array Object)에 저장됨.

### Uniform
---
Shader 프로그램을 실행하기 전에 설정하는 전역 변수

### Texture
---
Texture는 Shader 프로그램에서 무작위 접근이 가능한 Data Array입니다. Texture에 넣는 가장 일반적인 데이터는 Image Data지만 Texture는 단순히 Data일 뿐이므로, 색상 이외에 다른 데이터도 저장할 수 있음.

### Varing 
---
Varing은 Vertext Shader에서 Fragment Shader로 데이터를 전달하기 위한 방안입니다.
렌더링되는것이 점인지, 선인지, 또는 삼각형인지에 따라 Vertex에서 varying에 설정한 값은 Fragment Shader를 실행하는 중에 보간됩니다.


## WebGL Hello World
---
WebGL에서 중요한 것은 Clip Space Coordinate와 Color임.
WebGL을 사용하는 프로그래머로서 할 일은 이 두 가지를 WebGL에 제공하는 것.
이 두가지 정보를 제공하기 위한 두 개의 'Shader'를 제공
- Vertex Shader -  Clip Space Coordinate
- Fragment Shader - Color

Clip Space Coordinate는 캔버스 크기에 상관 없이 항상 -1 ~ 1의 범위를 가짐

### Example - Vertex Shader 
---
``` cpp
#version 300 es

// attribute는 정점셰이더에 대한 입력.
// 버퍼로부터 데이터를 받음
in vec4 a_position;

- // 모든 셰이더는 main 함수를 가지고 있습니다.
void main(){
	
	// gl_Position은 정점 셰이더가 설정해 주어야 하는 내장 변수입니다.
	gl_position = a_position;
}


```

### Example - Fragment Shader 
---
``` cpp
#version 300 es

// 프래그먼트 셰이더는 기본 정밀도를 가지고 있지 않으므로 선언을 해야합니다.
// highp가 기본값으로 적당함 "높은 정밀도(High Precision)"을 의미함.
precision highp float;

// 프래그먼트 셰이더는 출력값을 선언해야 합니다.
out vec4 outColor;

void main(){
	// 붉은-보라색 상수로 출력값을 설정합니다.
	outColor = vec4(1, 0, 0.5, 1);
}

```

위에서 프래그먼트 셰이더의 출력으로 `outColor`를 선언했습니다. `outColor`를 `1, 0, 0.5, 1`로 설정했는데, `r, g, b, a`를 의미하며, 각 값은 `0 ~ 1` 값을 사용함.


이제 두 개의 Shader 함수를 작성했으니 WebGL 부분을 시작해 봅시다.

### WebGL Rendering 
---

첫 번째로 HTML 캔버스(canvas) 요소가 필요합니다.

``` html
<canvas id="c"></canvas>
```

JavaScript에서 Canvas를 찾는다.
``` js
var canvas = document.querySelector("#c");
```

이제 WebGL2RenderingContext를 생성할 수 있다.

```js
var gl = canvas.getContext("webgl2");
if(!gl) {
	// webgl2를 사용할 수 없습니다!
}
```

Shader Program을 GPU에 넣기 위해 컴파일해야 하므로 먼저 Shader를 문자열로 가져와야 합니다.
 JavaScript String 문법을 활용해 GLSL 문자열을 생성한다.
  
 - AJAX를 사용하여 다운로드
 - Script 태그에 삽입
 - Multiline Template 문자열에 삽입

```js
var vertexShaderSource = `#version 300 es

// attribute는 vertex shader에 대한 입력(in)입니다.
// receive data from 'buffer'
in vec4 a_postion;

// every shader has a 'main' function
void main() {
	// gl_position은 vertex shader가 설정해주어야 함
	gl_postion = a_position;
}
`;

var fragmentShaderSource = `#version 300 es
precision highp flaot;

// fragment shaders must declare output values.
out vec4 outColor;

void main(){
	outColor = vec4(1, 0, 0.5, 1);
}
`;

```

사실 대부분 3D 엔진에서는 다양한 유형의 템플릿, 코드 연결(concatenation) 등의 기법을 사용하여 GLSL 셰이더들을 그때그때 생성합니다. 하지만 이 사이트의 예제들은 런타임에 GLSL을 생성해야 할만큼 복잡하지는 않습니다.

> [!주의]
> 주의: `#version 300 es`는 **반드시 첫 번째 라인에 작성해야 합니다**. 그 앞에 주석이나 빈 줄이 있으면 안됩니다! `#version 300 es`는 WebGL2에게 GLSL ES 3.00이라 부르는 셰이더 언어를 사용하라고 알려줍니다. 만약 이를 첫 번째 라인에 작성하지 않았다면 셰이더 언어는 기본값인 WebGL 1.0의 GLSL ES 1.00으로 설정이 되는데, 이 버전은 많은 차이점이 있고 기능도 훨씬 적습니다.



### GLSL Source Upload & Shader Compile
---
``` js
function createShader(gl, type, source) {
	var shader = gl.createShader(type);
	gl.shaderSource(shader, source);
	gl.compileShader(shader);
	var success = gl.getShaderParameter(shader, gl.COMPILE_STATUS);
	if(success){
		return shader;
	}
	 
	console.log(gl.getShaderInfoLog(shader));
	gl.deleteShader(shader);
}
```

위 코드의 결과로 이 함수를 호출하여 두 개의 셰이더를 생성할 수 있음.

``` js
var vertexShader = createShader(gl, gl.VERTEX_SHASDER, vertexShaderSource);
var fragmentShader = createShader(gl, gl.FRAGMENT_SHADER, fragmentShaerSource);
```

생성된 shader를 프로그램으로 Link

```js
function createProgram(gl, vertexShader, fragmentShader) {
	var program = gl.createProgram();
	gl.attachShader(program, vertexShader);
	gl.attachShader(program, fragmentShader);
	gl.linkProgram(program);
	var success = gl.getProgramParameter(program, gl.LINK_STATUS);
	if(success) {
		return program;
	}
	
	console.log(gl.getProgramInfoLog(program));
	gl.deleteProgram(program);
}
```

```js
var program = createProgram(gl, vertexShader, fragmentShader);
```

 - WebGL API: 우리가 만든 **GLSL Program**에 데이터를 **제공**하기 위한 **상태를 설정**

---

해당 예제에는 `a_position` attribute만 존재함.

```js
var postionAttributeLocation = gl.getAttributeLocation(program, "a_position");
```

- attribute의 location(or uniform의 location)을 찾는 것은 **초기화**에서 진행(**Render Loop✖**)

```js
var postionBuffer = gl.createBuffer();
```

### Bind Buffer
---
WebGL은 많은 WebGL 리소스들을 전역 바인드 포인트(bind point)를 통해 조작하도록 되어 있습니다. 바인드 포인트는 WebGL의 내부 전역 변수라고 생각하시면 됩니다. 먼저 리소스를 바인드 포인트에 바인드합니다. 그런 다음 다른 모든 함수들은 그 바인드 포인트를 통해 리소스를 참조합니다. positionBuffer를 바인드 해봅시다.

```js
gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
```

이후 Bind point를 통해 Buffer를 참조하므로써, Buffer에 데이터를 설정할 수 있음.

```js
var positions = [
0, 0,
0, 0.5,
0.7, 0,
];
gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(position), gl.STATIC_DRAW);
```


WebGL의 엄격한 형식의 데이터를 필요로 하므로  JS Array인 `positions`값을 `Flaot32Array(position)`을 사용하여, 32bit Flaot Array를 생성.
`gl.bufferData`는 그 배열을 GPU에 있는 `positionBuffer`에 복사한다.
위에서, `ARRAY_BUFFER`의 BindPoint `positionBuffer`를 Bind 해둔 상태이기 때문에 복사되는 것.

`gl.STATIC_DRAW`는 WebGL에 우리가 데이터를 어떻게 사용할 것인지에 대한 힌트를 제공
WebGL은 몇몇 상황들을 최적화 하기 위해 이 힌트를 사용함.

 - `gl.STATIC_DRAW` 데이터를 자주 변경하지 않음

### Get Data to Attribute
---

```js
var vao = gl.createVertexArray();
```
 
이후, 수행할 모든 Attribute 설정이 위 Attribute 상태 집합에 적용되도록 하기 위해, 사용중인 Vertex Array로 설정

```js 
gl.bindVertexArray(vao);
```

 먼저 attribute를 켜야 합니다. 이는 WebGL에게 우리가 버퍼에서 데이터를 가져오려고 한다는 것을 알려주는 것입니다. attribute을 켜지 않으면 attribute는 상수 값을 가지게 됩니다.
 
```js
gl.enableVertexAttribArray(positionAttributeLocation);

```

```js
var size = 2;         //iteration 마다 두개 구성 요소 사용
var type = gl.FLOAT;  // 데이터는 32bit 부동 소수점
var normalize = false; // 데이터를 정규화 하지 않음
var stride = 0; // 0인 경우, 실행할 때마다 'size' * sizeof(type)' 만큼 다음위치로 이동
var offset = 0; // 버퍼의 시작부터 데이터를 읽어옴
gl.vertexAttribPointer(
positionAttributeLocation, size, type, normalize, stride, offset)
```


그리기 전에 캔버스 크기를 디스플레이 크기와 일치하도록 조정 해야 합니다. 캔버스에는 이미지와 마찬가지로 두 가지 크기가 있습니다. 실제로 포함되어있는 픽셀의 수와 표시되는 크기가 따로 있습니다. CSS가 캔버스가 표시되는 크기를 결정합니다. 다른 방법보다 훨씬 유연하기 때문에 **항상 CSS를 사용해 원하는 캔버스 크기를 설정해야 합니다**.

캔버스가 표시되는 실제 크기와 픽셀 수를 일치 시키기 위해서 [여기에서 볼 수 있는 헬퍼 함수를 사용하고 있습니다](https://webgl2fundamentals.org/webgl/lessons/ko/webgl-resizing-the-canvas.html).

``` js
	webglUtils.resizeCanvasToDisplaySize(gl.canvas);
```


---
#vertexshader #fragmentshader #attribute #buffer #varying #uniform #GLSL #webgl


