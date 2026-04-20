GPU에서 사용되는 작은 프로그램
GLSL(OpenGL Shader Language)이라는 문법으로 작성됨
### Vertex Shader

Vertex Shader는 셰이더는 3D 객체의 각 정점에서 작동합니다. 
일반적으로 변환(크기 조정, 회전, 변환) 및 조명 계산과 같은 작업을 수행합니다. 
다음은 GLSL로 작성된 정점 셰이더의 간단한 예입니다.

``` cpp
attribute vec4 aVertexPosition;  
uniform mat4 uModelViewMatrix; uniform mat4 uProjectionMatrix;  

void main(void) 
{     
	gl_Position = uProjectionMatrix * uModelViewMatrix * aVertexPosition; 
}
```


- `attribute vec4 aVertexPosition`:  vertex position 속성을 입력합니다..
- `uniform mat4 uModelViewMatrix` and `uniform mat4 uProjectionMatrix`: 모델 뷰 및 투영 행렬에 대한 균일 변수.
- `gl_Position`: 화면 공간에서 정점의 최종 위치를 저장하는 내장 변수입니다.

### Fragment Shader

Fragment Shader는 각 픽셀의 색상과 기타 속성을 결정합니다. 
다음은 GLSL의 Fragment Shader에 대한 간단한 예입니다.

```cpp
precision mediump float;  
void main(void) {     
	gl_FragColor = vec4(1.0, 1.0, 1.0, 1.0); // White color 
}`
```

- `precision mediump float`: 부동 소수점 연산의 정밀도를 지정합니다.
- `gl_FragColor`: 조각의 최종 색상을 저장하는 내장 변수입니다.


```js
document.addEventListener("DOMContentLoaded", () => {
    const canvas = document.getElementById('glCanvas');
    const gl = canvas.getContext('webgl');

    if (!gl) {
        console.error('Unable to initialize WebGL. Your browser may not support it.');
        return;
    }

    gl.viewport(0, 0, canvas.width, canvas.height);
    gl.clearColor(0.0, 0.0, 0.0, 1.0);
    gl.clear(gl.COLOR_BUFFER_BIT);

    const vertexShaderSource = `
        attribute vec4 aVertexPosition;
        void main(void) {
            gl_Position = aVertexPosition;
        }
    `;

    const fragmentShaderSource = `
        void main(void) {
            gl_FragColor = vec4(1.0, 1.0, 1.0, 1.0);
        }
    `;

    function compileShader(gl, source, type) {
        const shader = gl.createShader(type);
        gl.shaderSource(shader, source);
        gl.compileShader(shader);

        if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
            console.error('An error occurred compiling the shaders: ' + gl.getShaderInfoLog(shader));
            gl.deleteShader(shader);
            return null;
        }
        return shader;
    }

    const vertexShader = compileShader(gl, vertexShaderSource, gl.VERTEX_SHADER);
    const fragmentShader = compileShader(gl, fragmentShaderSource, gl.FRAGMENT_SHADER);

    const shaderProgram = gl.createProgram();
    gl.attachShader(shaderProgram, vertexShader);
    gl.attachShader(shaderProgram, fragmentShader);
    gl.linkProgram(shaderProgram);

    if (!gl.getProgramParameter(shaderProgram, gl.LINK_STATUS)) {
        console.error('Unable to initialize the shader program: ' + gl.getProgramInfoLog(shaderProgram));
    }

    gl.useProgram(shaderProgram);

    const vertices = new Float32Array([
        0.0,  1.0,
       -1.0, -1.0,
        1.0, -1.0
    ]);

    const vertexBuffer = gl.createBuffer();
    gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer);
    gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW);

    const vertexPosition = gl.getAttribLocation(shaderProgram, 'aVertexPosition');
    gl.enableVertexAttribArray(vertexPosition);
    gl.vertexAttribPointer(vertexPosition, 2, gl.FLOAT, false, 0, 0);

    gl.drawArrays(gl.TRIANGLES, 0, 3);
});

```