```html
<!-- Licensed under a BSD license. See license.html for license -->
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>TTTTTTTest</title>
    <link type="text/css" href="index.css" rel="stylesheet" />
</head>

<body>
    <h1>WebGL TEST</h1>
    <canvas id="canvas"></canvas>
    <div id="uiContainer">
        <div id="ui">
            <div id="x"></div>
            <div id="y"></div>
        </div>
    </div>
</body>
<!--
for most samples webgl-utils only provides shader compiling/linking and
canvas resizing because why clutter the examples with code that's the same in every sample.
See https://webgl2fundamentals.org/webgl/lessons/webgl-boilerplate.html
and https://webgl2fundamentals.org/webgl/lessons/webgl-resizing-the-canvas.html
for webgl-utils, m3, m4, and webgl-lessons-ui.
-->
<script src="webgl-utils.js"></script>
<script src="webgl-lessons-ui.js"></script>
<script src="m3.js"></script>
<script>

    "use strict";

    var vs = /*glsl*/ ` #version 300 es

in vec2 a_position;
uniform vec2 u_resolution;
uniform vec2 u_translation;
uniform vec2 u_rotation;


void main() {
 vec2 position = a_position + u_translation;

  // convert the position from pixels to 0.0 to 1.0
  vec2 zeroToOne = position / u_resolution;

  // convert from 0->1 to 0->2
  vec2 zeroToTwo = zeroToOne * 2.0;

  // convert from 0->2 to -1->+1 (clipspace)
  vec2 clipSpace = zeroToTwo - 1.0;

  gl_Position = vec4(clipSpace * vec2(1, -1), 0, 1);
}
    `;

    var fs = ` #version 300 es
    precision highp float;

    uniform vec4 u_color;

    out vec4 outColor;

    void main() 
    {
        outColor = u_color;
    }
`;
    function main() {
        // Get A WebGL context
        /** @type {HTMLCanvasElement} */
        var canvas = document.querySelector("#canvas");
        var gl = canvas.getContext("webgl2");
        if (!gl) {
            return;
        }

        var program = webglUtils.createProgramFromSources(gl, [vs, fs]);

        // look up where the vertex data needs to go.
        var positionAttributeLocation = gl.getAttribLocation(program, "a_position");
        var resolutionUniformLocation = gl.getUniformLocation(program, "u_resolution");
        var colorLocation = gl.getUniformLocation(program, "u_color");
        var translationLocation = gl.getUniformLocation(program, "u_translation");

        // Create a buffer
        var positionBuffer = gl.createBuffer();
        gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);

        var vao = gl.createVertexArray();
        gl.bindVertexArray(vao);
        
        // and make it the one we're currently working with
        gl.enableVertexAttribArray(positionAttributeLocation);

        // Tell the attribute how to get data out of positionBuffer (ARRAY_BUFFER)
        var size = 2;          // 2 components per iteration
        var type = gl.FLOAT;   // the data is 32bit floats
        var normalize = false; // don't normalize the data
        var stride = 0;        // 0 = move forward size * sizeof(type) each iteration to get the next position
        var offset = 0;        // start at the beginning of the buffer
        gl.vertexAttribPointer(positionAttributeLocation, size, type, normalize, stride, offset);

        // set data 
        setGeometry(gl);
        var translation = [0, 0];
        var color = [Math.random(), Math.random(), Math.random(), 1];

        drawScene();

         webglLessonsUI.setupSlider("#x", {slide: updatePosition(0), max: gl.canvas.width});
         webglLessonsUI.setupSlider("#y", {slide: updatePosition(1), max: gl.canvas.height});

        function updatePosition(index) {
            return function (event, ui) {
                translation[index] = ui.value;
                drawScene();
            };
        }

        function drawScene() {
            webglUtils.resizeCanvasToDisplaySize(gl.canvas);
            gl.viewport(0, 0, canvas.width, canvas.height);
            gl.clearColor(0, 0, 0, 0);
            gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
            gl.useProgram(program);

            gl.bindVertexArray(vao);
            // 
            gl.uniform2f(resolutionUniformLocation, gl.canvas.width, gl.canvas.height);

            gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
      
            // set color
            gl.uniform4fv(colorLocation, color);
            // set translation
            gl.uniform2fv(translationLocation, translation);

            var primitiveType = gl.TRIANGLES;
            var offset = 0;
            var count = 18;
            gl.drawArrays(primitiveType, offset, count);
        }        
    }

    function setGeometry(gl) {
  gl.bufferData(
      gl.ARRAY_BUFFER,
      new Float32Array([
          // 왼쪽 열
          0, 0,
          30, 0,
          0, 150,
          0, 150,
          30, 0,
          30, 150,
 
          // 위 가로(rung)
          30, 0,
          100, 0,
          30, 30,
          30, 30,
          100, 0,
          100, 30,
 
          // 중간 가로(rung)
          30, 60,
          67, 60,
          30, 90,
          30, 90,
          67, 60,
          67, 90]),
      gl.STATIC_DRAW);
}
    main();
</script>

</html>
```



두 코드는 모두 2D 좌표 회전을 구현하고 있지만, 약간 다른 접근 방식을 사용하고 있습니다. 이 두 방식의 관계를 설명해 드리겠습니다.

1. 첫 번째 코드 (삼각함수 사용):
```python
theta = -math.pi / 6  # 회전각 (라디안)
x_new = x * math.cos(theta) - y * math.sin(theta)
y_new = x * math.sin(theta) + y * math.cos(theta)
```

2. 두 번째 코드 (회전 벡터 사용):
```glsl
rotatedX = a_position.x * u_rotation.y + a_position.y * u_rotation.x;
rotatedY = a_position.y * u_rotation.y - a_position.x * u_rotation.x;
```

이 두 코드의 관계는 다음과 같습니다:

1. 기본 원리:
   두 코드 모두 2D 회전 변환을 수행합니다. 첫 번째 코드는 직접적으로 삼각함수(sine, cosine)를 사용하고, 두 번째 코드는 미리 계산된 회전 벡터를 사용합니다.

2. 회전 표현 방식:
   - 첫 번째 코드: 회전각 θ를 직접 사용합니다.
   - 두 번째 코드: 회전을 (cos θ, sin θ) 형태의 벡터로 표현합니다. 여기서 u_rotation.y는 cos θ에, u_rotation.x는 sin θ에 해당합니다.

3. 수식의 대응 관계:
   두 번째 코드를 첫 번째 코드의 형태로 변환하면 다음과 같습니다:
   ```
   rotatedX = a_position.x * cos(θ) + a_position.y * sin(θ)
   rotatedY = a_position.y * cos(θ) - a_position.x * sin(θ)
   ```
   
   이는 첫 번째 코드와 거의 동일하지만, sin θ의 부호가 반대입니다. 이는 회전 방향의 차이를 나타냅니다.

4. 회전 방향:
   - 첫 번째 코드: 음수 각도를 사용하여 시계 방향 회전을 구현합니다.
   - 두 번째 코드: 양수 각도를 가정하여 반시계 방향 회전을 구현합니다.

5. 최적화:
   두 번째 코드는 그래픽스 프로그래밍(예: WebGL)에서 자주 사용되는 방식입니다. 회전 벡터(u_rotation)를 미리 계산하여 전달함으로써, 각 정점에 대해 매번 삼각함수를 계산하는 비용을 줄일 수 있습니다.

6. 사용 컨텍스트:
   - 첫 번째 코드: 일반적인 프로그래밍 환경에서 단일 점의 회전에 적합합니다.
   - 두 번째 코드: 그래픽스 셰이더에서 많은 수의 정점을 효율적으로 회전시키는 데 적합합니다.

결론적으로, 두 코드는 같은 수학적 원리를 바탕으로 하지만, 구현 방식과 최적화 측면에서 차이가 있습니다. 두 번째 코드는 첫 번째 코드의 최적화된 버전으로 볼 수 있으며, 특히 그래픽스 프로그래밍 상황에 맞게 조정된 것입니다.