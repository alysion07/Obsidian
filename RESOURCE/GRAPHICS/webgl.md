--- 
[OpenGL ES](https://www.khronos.org/opengles/)

---

WebGL2는 WebGL1과 거의 100% 역호환이 된다.

WebGL은 사용자의 GPU에서 실행됨. 즉 GPU에서 실행되는 코드를 제공되어야 하며, 그 코드는 두 개 함수 쌍 형태로 제공되어야함.
 각각의 함수는 정점 셰이더(vertex shader)와 프래그먼트 셰이더(fragment shader)로 불린다.
 [[Shader]]는 C/C++과 유사한 **GLSL(GL Shader Language)** 로 작성되어야 함. 
 이 한쌍을 합쳐서 **프로그램**이라고 부른다.


## Vertex Shader
---
정점 셰의더의 역할은 정점의 위치를 계산하는것. 함수를 통해 출력 위치가 계산되고, WebGL은 그 위치를 기준으로 'Point', 'Line', 'Triangle'을 비롯한 다양한 종류의 기본요소(Primitives)를 **래스터화** 할 수 있다. 

## Fragment Shader
---
이 기본요소들을 래스터화 할 때 두 번째로 제공한 Fragment Shader를 호출함.  Fragment Shader의 역할은 현재 그려지고 있는 기본요소의 각 픽셀 색상을 계산한다는 것.

거의 모든 WebGL API는 이러한 함수 쌍을 실행하기 위한 상태를 설정하기 위해 존재 함.
그리려는 요소마다 다양한 상태를 설정해 준 다음, GPU에서 Shader를 실행하는 `gl.drawArrays` 또는 `gl.drawElements`를 호출해서 함수 쌍을 실행한다.

## Buffer
---
버퍼는 GPU에 올라가는 Binary Data Array 임. 물론 어떤 데이터든 원하는대로 넣을 수 있음. 일반적으로는 Position, Normal, Texture Coordinate, Vertex Color 등등의 항목 포함.

## Attribute 
---
Attribute는 어떻게 버퍼에서 데이터를 가져올지, 그리고 정점 