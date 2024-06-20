GPU에서 사용되는 작은 프로그램
GLSL(OpenGL Shader Language)이라는 문법으로 작성됨
### Vertex Shader

Vertex Shaders 셰이더는 3D 객체의 각 정점에서 작동합니다. 일반적으로 변환(크기 조정, 회전, 변환) 및 조명 계산과 같은 작업을 수행합니다. 다음은 GLSL로 작성된 정점 셰이더의 간단한 예입니다.

``` cpp
attribute vec4 aVertexPosition;  
uniform mat4 uModelViewMatrix; uniform mat4 uProjectionMatrix;  

void main(void) 
{     
	gl_Position = uProjectionMatrix * uModelViewMatrix * aVertexPosition; 
}
```


- `attribute vec4 aVertexPosition`: Input vertex position attribute.
- `uniform mat4 uModelViewMatrix` and `uniform mat4 uProjectionMatrix`: Uniform variables for the model-view and projection matrices.
- `gl_Position`: Built-in variable to store the final position of the vertex in screen space.

### Fragment Shader

A fragment shader determines the color and other attributes of each pixel. Here’s a simple example of a fragment shader in GLSL:

```cpp
precision mediump float;  
void main(void) {     
	gl_FragColor = vec4(1.0, 1.0, 1.0, 1.0); // White color 
}`
```

- `precision mediump float`: Specifies the precision of floating-point operations.
- `gl_FragColor`: Built-in variable to store the final color of the fragment.