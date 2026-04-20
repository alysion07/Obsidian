
|        Aspect        | Attribute                                                                              | Uniform                                                                          |
| :------------------: | :------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------- |
|          목적          | 위치, 색상, 텍스처 좌표와 같은 정점별 데이터를 전달하는데 사용                                                   | 특정 그리기 호출(ex. 변환 행렬, 조명 정보)에 대해 일정하게 유지되는 데이터를 전달하는데 사용                          |
|        변경 빈도         | 정점당 변경 사항. 각 정점은 서로 다른 속성 값을 가질 수 있다.                                                  | 그리기 호출에 대해 일정하게 유지됨.(즉, 각 정점에 대해 변경되지 않음)                                        |
|        셰이더유형         | **Vertex Shader**에서만 사용할 수 있다.                                                         | Vertex 및 Fragment Shader에서 모두 사용 가능                                              |
|      Data type       | 일반적으로 백터(vec2, 3,4) 또는 행렬이 사용된다.                                                       | 스칼라(float, int), 백터(vec2, vec3), 또는 행렬(mat4)                                     |
|       use case       | Vertex Position, normal, color, texture coordinate                                     | transform matrix, light parameter, meterial property                             |
|         제한사항         | 특정 갯수로 제한됨(하드웨어에 따라 다르며, 일반적으로 16개).                                                   | uniform도 제한이 있지만 일반적으로 attribute 보다 더 많은 수를 사용할 수 있다.                            |
|     shader에서 엑세스     | Vertex shader에서 정점별로 설정해야 함. fragment shader에서 직접 사용할 수 없다.                            | Vertex 및 Fragment Shader 모두 엑세스 가능. 단일 그리기 호출의 두 shader에서 모두 유지된다.               |
| WebGL에서 데이터를 전달하는 기능 | `gl.vertexAttribPointer()` 및 `gl.enableVertexAttribArray()`는 Attribute data를 전달하는 데 사용 | `gl.uniformXf()`(여기서 X는 유형, 예: `1f`, `3f`, `Matrix4fv`)는 uniform data를 전달하는 데 사용 |


## Attribute

#attribute

어플리케이션(JavaScript)에서 **vertex shader**로 데이터를 전달하는데 사용됨. 이 값은 vertex마다 다르다. 즉 모든 vertex가 서로 다른 attribute data를 가질 수 있다.
 - vertex position
 - vertex color
 - texture coordinate

```glsl
attribute vec3 aPosition;
attribute vec3 aColor;
```

JavaScript 코드에서는 `gl.svertexAttribPointer()` 및 `gl.enableVertexAttribArray()`와 같은 함수를 사용하여 attribute를 설정한다. 이 함수들은 buffer data(pos, color)를 shader의 attribute에 바인딩한다.

``` js
const position = gl.getAttribLocation(program, 'aPosition');
gl.vertexAttribPointer(position, 3, gl.FLOAT, flase, 0, 0);
gl.enableVertexAttribArray(position);
```

## Uniform 

#uniform

 **"전체 그리기 호출에 대한 전역 상수"**

uniform은 특정 그리기 호출에서 모든 vertex와 fragment에 걸쳐 일정하게 유지되는 data를 전달하는데 사용된다. 이러한 값은 각 vertex에 대해 변경되지 않지만 drawing process 중에 모든 vertex 또는 fragment에서 전역적으로 공유된다.

```glsl
uniform mat4 uModelViewMatrix;
uniform mat4 uProjectionMatrix;
uniform vec3 uLightPosition;
```

`gl.uniformXf()` 또는 `gl.uniformMatrixXfv()`를 사용하여 uniform value를 설정

```js
const uModelViewMatrix = gl.getUniformLocation(program, 'uModelViewMatrix');
gl.uniformMatrix4fv(uModelViewMatrix, false, modelViewMatrix);
```

이 행렬은 단일 그리기 호출의 모든 vertex에 대해 동일하게 유지되므로 전체 객체가 동일한 방식으로 변환됩니다.