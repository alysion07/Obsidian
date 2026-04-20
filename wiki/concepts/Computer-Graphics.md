---
title: Computer Graphics
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 그래픽스
  - 렌더링
  - WebGL
  - 셰이더
---

# Computer Graphics

컴퓨터 그래픽스의 핵심 개념들을 정리한 종합 페이지. 렌더링 파이프라인, 셰이더, 버퍼 객체, 텍스처링, 좌표 변환, 투명도 처리, 수학 기초, WebGPU까지 포괄한다.

---

## 1. Rendering Pipeline 개요

전형적인 그래픽스 렌더링 파이프라인은 다음과 같은 흐름을 따른다.

**초기화 단계 (Init-time)**
1. Shader를 생성하고 Program을 만든다. Uniform/Attribute의 location을 탐색한다.
2. Buffer를 생성하고 vertex data를 업로드한다.
3. Vertex Array를 생성하여 각 attribute에 대해 바인딩과 포인터 설정을 수행한다.
4. Texture를 생성하고 데이터를 업로드한다.

**렌더링 단계 (Render-time)**
1. Canvas를 클리어하고 viewport, depth testing, culling 등 전역 상태를 설정한다.
2. 각 오브젝트에 대해 shader program 사용, vertex array 바인딩, uniform 설정을 수행한다.
3. `gl.drawArrays` 또는 `gl.drawElements`를 호출하여 GPU에서 셰이더를 실행한다.

Graphics Library(GL) API는 크게 세 종류로 나뉜다:
- GPU에 **데이터를 전송**하는 API
- 라이브러리의 **설정을 변경**하는 API
- GPU에 **작업을 명령**하는 API

데이터 전송과 드로우 명령을 분리한 이유는 CPU-GPU 간 병목 현상을 방지하기 위함이다.

---

## 2. Shader

Shader는 GPU에서 실행되는 작은 프로그램으로, GLSL(OpenGL Shader Language)로 작성된다.

### Vertex Shader

Vertex Shader는 3D 객체의 각 정점(vertex)에 대해 실행되며, ClipSpace 좌표를 생성하는 것이 주된 역할이다. 변환(이동, 회전, 스케일)과 조명 계산을 수행한다.

```glsl
attribute vec4 aVertexPosition;
uniform mat4 uModelViewMatrix;
uniform mat4 uProjectionMatrix;

void main(void) {
    gl_Position = uProjectionMatrix * uModelViewMatrix * aVertexPosition;
}
```

- `gl_Position`: 화면 공간에서 정점의 최종 위치를 저장하는 내장 변수

Vertex Shader에 데이터를 전달하는 3가지 방법:
1. **Attributes** - 버퍼에서 가져온 정점별 데이터
2. **Uniforms** - draw call 마다 모든 정점에서 동일한 값
3. **Textures** - 픽셀/텍셀 데이터

### Fragment Shader

Fragment Shader는 래스터화 과정에서 각 픽셀의 색상을 결정한다. 픽셀당 한 번 호출된다.

```glsl
precision mediump float;
void main(void) {
    gl_FragColor = vec4(1.0, 1.0, 1.0, 1.0); // White color
}
```

WebGL2에서는 `gl_FragColor` 대신 `out` 키워드를 사용하여 여러 버퍼로 출력이 가능하다.

### Varying

Varying은 Vertex Shader에서 Fragment Shader로 데이터를 전달하는 방법이다. 렌더링 대상(점, 선, 삼각형)에 따라 Vertex Shader에서 설정한 값이 Fragment Shader 실행 중에 보간(interpolate)된다.

---

## 3. Deferred Shading / Rendering

> 상세 내용: [[Deferred-Rendering]]

기존의 Forward Rendering은 Scene에 있는 모든 광원에 따라 각 객체를 개별적으로 렌더링하고 조명을 적용하는 방식이다. 이해와 구현은 쉽지만 광원이 많아질수록 성능이 급격히 저하된다.

Deferred Shading은 이를 개선한 기법으로, 기하 정보(G-Buffer)를 먼저 렌더링한 뒤 조명 계산을 후처리 단계에서 수행한다.

---

## 4. Buffer Objects

### VBO (Vertex Buffer Object)

VBO는 대량의 정점 데이터를 GPU 메모리에 저장하는 버퍼 객체이다. CPU에서 GPU로의 데이터 전송은 비교적 느리므로, 가능한 많은 데이터를 한 번에 전송해야 한다. 데이터가 GPU 메모리에 올라가면 Vertex Shader가 거의 즉각적으로 접근할 수 있다.

```c
unsigned int VBO;
glGenBuffers(1, &VBO);
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```

`glBufferData`의 usage 옵션:
- `GL_STATIC_DRAW` - 데이터가 거의 변하지 않음
- `GL_DYNAMIC_DRAW` - 데이터가 자주 변경됨
- `GL_STREAM_DRAW` - 매 프레임 변경됨

### VAO (Vertex Array Object)

VBO는 정점 데이터를 단순한 바이트 배열로 저장할 뿐 내부 구조를 알지 못한다. VAO는 VBO의 데이터를 OpenGL이 해석하고 사용할 수 있게 해주는 객체이다.

VAO에는 두 종류의 슬롯이 존재한다:
- **속성(Attribute)** 슬롯 - 0번은 위치, 1번은 색상 등으로 할당
- **VBO 바인딩** 슬롯 - VBO를 연결하는 슬롯

VAO의 사용 목적은 여러 Vertex Buffer에 대해 매번 Layout을 개별 지정할 필요를 없애는 것이다. 편의성과 성능 면에서 우월하며, OpenGL이 권장하는 방식이다.

VAO 설정 흐름:
1. VAO 생성 및 바인딩
2. VBO를 VAO 슬롯에 바인딩 (`glVertexArrayVertexBuffer`)
3. Attribute 활성화 (`glEnableVertexArrayAttrib`)
4. Attribute와 VBO 연결 (`glVertexArrayAttribBinding`)
5. VBO Format 설정 (`glVertexArrayAttribFormat`)

### FrameBuffer

FrameBuffer는 화면에 그려질 전체 화면 정보를 담는 메모리 공간이다. 그래픽카드는 FrameBuffer 메모리를 읽어 화면을 표시하며, 1초에 몇 번 갱신되는지를 주사율(Refresh Rate, Hz)이라 한다.

**더블 버퍼링 (Double Buffering)**: Front Buffer(현재 표시 중)와 Back Buffer(그리는 중)를 사용하여 완성된 프레임만 표시하는 기법. Back Buffer가 완성되면 Front Buffer와 Swap한다.

**스크린 테어링 (Screen Tearing)**: Buffer Swap 중 디스플레이에 전송되어 서로 다른 프레임이 혼합 표시되는 현상. **V-Sync**(수직 동기화)를 통해 FPS를 디스플레이 주사율에 고정하여 해결한다.

### Depth Buffer (Z-Buffer)

Depth Buffer는 각 픽셀의 깊이 값(Z값)을 저장하는 버퍼이다. Z값은 ClipSpace(-1 ~ +1)으로 변환되며, 뒤쪽 픽셀이 앞쪽 픽셀을 덮어쓰지 않도록 하는 것이 주 용도이다.

```js
gl.enable(gl.DEPTH_TEST);
gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
```

Three.js에서는 깊이 값이 0~1로 정규화되며, 0에 가까울수록 카메라에 가까운 객체이다.

---

## 5. Texturing

### Attribute와 Uniform

| 구분 | Attribute | Uniform |
|------|-----------|---------|
| 목적 | 위치, 색상, 텍스처 좌표 등 정점별 데이터 전달 | 변환 행렬, 조명 정보 등 draw call 전체에서 일정한 데이터 전달 |
| 변경 빈도 | 정점마다 변경 | draw call 전체에서 일정 |
| 셰이더 사용 | Vertex Shader에서만 사용 | Vertex 및 Fragment Shader 모두 사용 가능 |
| 설정 함수 | `gl.vertexAttribPointer()`, `gl.enableVertexAttribArray()` | `gl.uniformXf()`, `gl.uniformMatrixXfv()` |

### Texture Mipmaps

Mipmap은 미리 계산된 일련의 텍스처로, 원본을 점진적으로 낮은 해상도로 표현한 것이다. 1/4씩 크기가 줄어들며, 쌍선형 보간(Bilinear interpolation)으로 생성된다.

텍스처 필터링 옵션:
- `NEAREST` / `LINEAR` - 가장 큰 밉맵에서 1개 또는 4개 픽셀 선택
- `NEAREST_MIPMAP_NEAREST` - 적절한 밉맵 선택 후 1개 픽셀
- `LINEAR_MIPMAP_LINEAR` - 가장 적절한 두 밉맵에서 각 4개 픽셀 블렌딩 (가장 고품질, 가장 느림)

밉맵은 추가 메모리(33%)를 사용하지만 아티팩트(앨리어싱, 모아레 패턴, 텍스처 스위밍, 밴딩 등)를 줄이는 데 효과적이다.

### Cubemap

Cubemap은 큐브의 6면을 형성하는 2D 텍스처들을 하나로 묶은 텍스처이다. **방향 벡터**를 사용해 인덱싱/샘플링할 수 있다는 것이 핵심 장점으로, 방향만 제공하면 GL이 해당 방향과 맞닿는 텍셀을 반환한다.

---

## 6. Coordinate Systems (ClipSpace 좌표 변환)

픽셀 좌표를 WebGL의 ClipSpace 좌표(-1 ~ +1)로 변환하는 과정:

```
1. zeroToOne = position / u_resolution      // 픽셀 -> 0.0~1.0 정규화
2. zeroToTwo = zeroToOne * 2.0               // 0.0~1.0 -> 0.0~2.0 확장
3. clipSpace = zeroToTwo - 1.0               // 0.0~2.0 -> -1.0~+1.0 변환
4. gl_Position = vec4(clipSpace * vec2(1, -1), 0, 1)  // Y축 반전
```

Y축을 반전하는 이유는 WebGL 좌표계(위가 +y)와 일반적인 2D 그래픽스 좌표계(아래가 +y)의 차이를 조정하기 위함이다.

행렬을 사용한 변환 순서: Projection -> Translation -> Rotation -> Scale

---

## 7. Transparency Rendering

투명한 물체를 올바르게 렌더링하려면 그리는 순서가 중요하다:

- **불투명 객체**: 앞에서 뒤로(front-to-back) 정렬하여 그린다. DEPTH_TEST 덕분에 뒤에 가려진 픽셀의 Fragment Shader 실행을 건너뛸 수 있어 성능이 향상된다.
- **투명 객체**: 뒤에서 앞으로(back-to-front) 정렬하여 그린다. 뒤의 색상이 먼저 그려져야 투명도 블렌딩이 올바르게 작동한다.

대부분의 3D 엔진은 불투명 객체 리스트와 투명 객체 리스트를 별도로 관리하며, 오버레이나 후처리 효과를 위한 추가 리스트를 두기도 한다.

---

## 8. Math (수학 기초)

### 벡터의 내적 (Dot Product)

내적은 같은 차원의 두 벡터의 각 성분을 곱한 후 더해 스칼라를 만드는 연산이다.

```
u = (a, b), v = (c, d)
u . v = a*c + b*d
u . v = |u| * |v| * cos(theta)
```

내적의 활용:
- **앞뒤 판별**: u . v > 0이면 앞, = 0이면 옆, < 0이면 뒤
- **시야 판별**: u . v >= cos(beta/2)이면 시야 범위 내
- **램버트 코사인 법칙 (N dot L)**: 표면 법선(N)과 빛 방향(L)의 내적으로 Diffuse Shading 구현

코사인 함수를 직접 계산하는 것은 비용이 크므로, 내적을 사용하면 곱셈과 덧셈으로 대체할 수 있다.

### 벡터의 외적 (Cross Product)

외적은 3차원 벡터에서만 사용 가능하며, 두 벡터에 모두 수직인 법선 벡터를 구하는 연산이다.

```
a x b = [(y1*z2 - y2*z1), (z1*x2 - z2*x1), (x1*y2 - x2*y1)]
|a x b| = |a| * |b| * sin(theta)
```

외적의 활용:
- **평행 판별**: 외적 결과가 0이면 두 벡터는 평행
- **좌우 판별**: up . (a x b) > 0이면 오른쪽, < 0이면 왼쪽 (왼손 좌표계)
- 외적 결과 벡터의 길이 = 두 벡터가 이루는 평행사변형의 넓이

### 선형대수학

**선형 변환**: 그리드 선을 평행하고 일정한 간격으로 유지하는 변환
- 조건: 모든 선은 직선이어야 하며, 원점은 고정
- 기저벡터(Basis Vector): i-hat(x축), j-hat(y축), k-hat(z축)의 단위 벡터

**행렬식(Determinant)**: 정방행렬에서 정의되며, 행렬의 가역성을 판별. det(A) = ad - bc (2x2)
- 부호는 방향 보존 여부를 나타냄

**핵심 개념들:**
- **Rank**: 변환 결과의 차원 수 (Full Rank = 최대 차원 유지)
- **Column Space**: 행렬의 가능한 결과의 집합
- **Null Space (Kernel)**: 원점으로 이동되는 벡터들의 집합
- **Inverse Matrix**: det != 0일 때 역행렬이 존재
- **Span**: 주어진 벡터 조합으로 나타낼 수 있는 결과 벡터들의 집합
- **선형 독립/종속**: 각 벡터가 Span에 새 차원을 추가하는지 여부

---

## 9. WebGPU 개요

WebGPU는 차세대 웹 그래픽스 API로, 기존 WebGL보다 더 낮은 수준의 하드웨어 접근 권한을 제공한다. DirectX 12, Metal, Vulkan과 같은 현대적 그래픽스 API를 기반으로 설계되었으며, Compute Shader를 지원한다.

---

## 10. 기타 개념

### Object3D (Three.js)

Three.js 대부분의 객체에 대한 기본 클래스로, 3D 공간에서 객체를 조작하기 위한 속성 및 메서드를 제공한다. `.add()` 메서드로 자식 객체를 추가할 수 있으나, 그룹핑에는 `Group`을 사용하는 것이 권장된다.

### Artifact (아티팩트)

렌더링 중 발생하는 시각적 왜곡 또는 오류:
- **앨리어싱**: 객체 가장자리의 들쭉날쭉한 현상
- **모아레(Moire) 패턴**: 미세한 텍스처 렌더링 시 나타나는 불필요한 패턴
- **텍스처 스위밍**: 카메라 이동 시 텍스처가 부자연스럽게 움직이는 현상
- **밴딩**: 부드러운 그라데이션 대신 나타나는 색상 밴드

Mipmap을 사용하면 거리와 각도에 따라 적절한 해상도의 텍스처를 사용하여 이러한 아티팩트를 줄일 수 있다.

---

## Related Pages

- [[Deferred-Rendering]] - Deferred Rendering/Shading 상세
- [[WebGL2]] - WebGL2 학습 시리즈 정리
- [[RS-Engine]] - RS-Engine 프로젝트 (Deferred Rendering 구현)
