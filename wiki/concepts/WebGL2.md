---
title: WebGL2
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - WebGL
  - 그래픽스
  - JavaScript
---

# WebGL2

WebGL2는 브라우저에서 GPU를 활용한 2D/3D 그래픽스를 렌더링하기 위한 JavaScript API이다. OpenGL ES 3.0을 기반으로 하며, WebGL1과 거의 100% 역호환된다. GLSL ES 3.00 셰이더 언어를 사용한다.

---

## 핵심 구조

WebGL은 GPU에서 실행되는 두 개의 함수 쌍(Vertex Shader + Fragment Shader)을 제공하는 것이 핵심이다. 거의 모든 WebGL API는 이 함수 쌍을 실행하기 위한 상태 설정을 위해 존재한다.

- **Vertex Shader**: 정점의 ClipSpace 좌표를 계산. 정점당 한 번 호출
- **Fragment Shader**: 래스터화되는 각 픽셀의 색상을 계산. 픽셀당 한 번 호출
- **Buffer**: GPU에 올라가는 Binary Data Array (Position, Normal, Texture Coordinate, Color 등)
- **Attribute**: 버퍼에서 Vertex Shader로 데이터를 전달하는 방법을 정의
- **Uniform**: draw call 전체에서 모든 정점/픽셀에 동일하게 유지되는 전역 변수
- **Varying**: Vertex Shader에서 Fragment Shader로 보간된 데이터를 전달
- **Texture**: 셰이더에서 무작위 접근 가능한 데이터 배열

ClipSpace 좌표는 캔버스 크기에 상관없이 항상 -1 ~ +1 범위를 가진다 (Y up 좌표계, 오른손 법칙).

---

## 학습 시리즈 목차 (26 Chapters)

### 기초 (Ch.1~6)
| Chapter | 주제 | 핵심 내용 |
|---------|------|----------|
| 1 | WebGL 기초 | WebGL2 컨텍스트 생성, 셰이더 컴파일/링크, 프로그램 구조 |
| 2 | Canvas Resizing | CSS와 캔버스 크기 일치, clientWidth/clientHeight 활용 |
| 3 | 작동 원리 | WebGL 내부 동작 메커니즘 |
| 4 | Shader & GLSL | GLSL 문법, 데이터 타입(vec, mat), swizzle, 내장 함수 |
| 5 | Image Processing | 텍스처 기반 이미지 처리 기초 |
| 6 | Image Processing Continued | 고급 이미지 처리 기법 |

### 2D 변환 (Ch.7~10)
| Chapter | 주제 | 핵심 내용 |
|---------|------|----------|
| 7 | 2D Translation | 이동 변환 구현 |
| 8 | 2D Rotation | 회전 변환 구현 |
| 9 | 2D Scale | 크기 변환 구현 |
| 10 | Matrix | 변환 행렬 통합 (Projection -> Translation -> Rotation -> Scale 순서) |

### 3D 렌더링 (Ch.11~15)
| Chapter | 주제 | 핵심 내용 |
|---------|------|----------|
| 11 | 3D Orthographic | 직교 투영 |
| 12 | 3D Perspective | 원근 투영 (Near, Far, FOV, aspect) |
| 13 | Camera | 카메라 행렬, view matrix (카메라 행렬의 역행렬) |
| 14 | Matrix Naming | 행렬 명명 규칙 |
| 15 | Animation | 애니메이션 구현 |

### 텍스처 (Ch.16~16.2)
| Chapter | 주제 | 핵심 내용 |
|---------|------|----------|
| 16 | Texture | 텍스처 기초, 밉맵, 필터링 |
| 16.1 | Texture Unit | 텍스처 유닛 관리 |
| 16.2 | Texture Data | 텍스처 데이터 형식 |

### 씬 관리 (Ch.17~19)
| Chapter | 주제 | 핵심 내용 |
|---------|------|----------|
| 17 | Draw Multiple Things | 다수 오브젝트 렌더링 |
| 18 | Scene Graph | 씬 그래프 구조 |
| 19 | Text Rendering | 텍스트 렌더링 기법 |

### 조명 (Ch.20~20.2)
| Chapter | 주제 | 핵심 내용 |
|---------|------|----------|
| 20 | Lighting (Directional) | 방향성 조명, N dot L, Inverse Transpose Matrix |
| 20.1 | Point Lighting | 점 광원 |
| 20.2 | Spot Lighting | 스포트라이트 |

### 고급 기법 (Ch.21~26)
| Chapter | 주제 | 핵심 내용 |
|---------|------|----------|
| 21 | OBJ File Load | 3D 모델 파일 로딩 |
| 22 | Perspective Correct Texture Mapping | 원근 보정 텍스처 매핑 |
| 23 | Planar/Perspective Projection Mapping | 투영 매핑 기법 |
| 24 | Texture Rendering | 텍스처 렌더링 (Render-to-Texture) |
| 25 | Cubemap | 큐브맵 텍스처, 환경 매핑 |
| 26 | Picking | 오브젝트 선택(피킹) 기법 |

---

## 주요 학습 포인트

### Shader & GLSL

GLSL은 래스터화 그래픽스를 위한 수학적 계산 수행에 특화된 언어이다. 엄격한 타입 시스템을 가지며, vec/mat 타입에 대한 swizzle 접근을 지원한다.

```glsl
vec4 a = vec4(1, 2, 3, 4);
v.yyyy == vec4(v.y, v.y, v.y, v.y)  // swizzle
v.bgra == vec4(v.b, v.g, v.r, v.a)  // 색상 성분으로도 접근 가능
```

`#version 300 es`는 반드시 셰이더 코드의 첫 번째 라인에 위치해야 한다.

### Matrix 변환

2D/3D 변환은 행렬 곱셈으로 통합된다. 변환 순서: Projection -> Translation -> Rotation -> Scale.

`canvas.clientWidth`와 `canvas.clientHeight`를 사용하여 투영 행렬의 화면 비율을 계산하는 것이 기술적으로 더 정확하다.

### Lighting

Directional Lighting은 표면 법선(Normal)과 빛 방향 벡터의 내적으로 계산한다. 오브젝트에 비균등 스케일이 적용된 경우, normal 변환에 world matrix의 inverse transpose를 사용해야 법선이 올바르게 유지된다.

렌더링 파이프라인 요약:
1. Near, Far, FOV, aspect로 projection matrix 계산
2. Camera matrix 계산 후 역행렬로 view matrix 생성
3. Projection x View로 원근감이 적용된 뷰 계산
4. World matrix(move, rotation, scale) 적용
5. Normal에도 world matrix 반영 후 조명 효과 구현

---

## Related Pages

- [[Computer-Graphics]] - 컴퓨터 그래픽스 종합 개념
- [[Deferred-Rendering]] - Deferred Rendering 기법
