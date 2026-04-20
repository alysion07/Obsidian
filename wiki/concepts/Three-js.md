---
title: Three.js
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - Three.js
  - 3D
  - WebGL
  - JavaScript
---

# Three.js

## 개요

Three.js는 WebGL을 기반으로 브라우저에서 3D 그래픽을 렌더링할 수 있게 해주는 JavaScript 라이브러리이다. Geometry, Material, Light, Scene Graph 등의 개념을 추상화하여 3D 콘텐츠 제작을 용이하게 한다.

## Geometry

Geometry는 3D 객체의 형상을 정의한다. 내부 속성 데이터(vertex positions, normals, UVs 등)로 구성되며, BoxGeometry, SphereGeometry 등 다양한 기본 지오메트리를 제공한다.

## Transform

### Matrix 변환 순서

변환 행렬은 **Scale > Rotation > Translation (SRT)** 순서로 적용해야 올바르게 동작한다. 변환 행렬은 교환법칙이 성립하지 않기 때문이다.

```ts
box.position.set(0, 2, 0);
box.rotation.x = degToRad(45);
box.scale.set(2, 2, 2);
```

Three.js 내부 함수를 사용하면 코드 작성 순서와 무관하게 SRT 순서로 처리해준다.

### 부모-자식 관계

자식 객체는 부모 객체의 변환 영향을 받는다.

```ts
const parent = new THREE.Mesh(geoParent, material);
parent.position.y = 2;
parent.rotation.z = degToRad(45);

const child = new THREE.Mesh(geoChild, material);
child.position.x = 3;

parent.add(child);  // 자식으로 설정
scene.add(parent);
```

## Object3D

Three.js의 기본 3D 객체 클래스로, 모든 3D 객체의 부모 클래스이다.

## Scene Graph

Scene Graph는 3D 장면의 계층적 구조를 표현하는 트리 구조이다. 부모-자식 관계를 통해 변환이 전파된다.

## Material

### 주요 Material 종류

| Material | 특징 |
|----------|------|
| **MeshBasicMaterial** | 전달된 color 값 그대로 표현. Shading 없음 |
| **MeshLambertMaterial** | 확산 반사(diffuse) 표현 |
| **MeshStandardMaterial** | PBR 기반, displacementMap 등 지원 |
| **MeshPhysicsMaterial** | 물리 기반 렌더링 |
| **MeshToonMaterial** | 셀 셰이딩 효과 (젤다의 전설 스타일). HDRi 광원 사용 불가, mipmap 설정 필수 |

### MeshBasicMaterial 주요 속성

- `visible`: 가시화 여부
- `transparent`: opacity 속성 사용 여부
- `opacity`: 투명도 (0~1)
- `depthTest`: Z값 비교 연산 사용 여부
- `depthWrite`: Z값 저장 여부
- `side`: Culling 속성

### Line 종류

- **LineSegments**: Geometry Buffer를 2개씩 끊어 라인의 시작점/끝점으로 인식
- **LineLoop**: 첫 번째 인덱스와 마지막 인덱스를 이어 하나의 연결된 선으로 Draw

## Light

### 주요 광원 종류

- **AmbientLight**: Scene의 모든 Object를 균등하게 비추는 광원. 세기를 약하게(0.1) 설정하여 보조 광원으로 사용
- **HemisphereLight**: AmbientLight와 달리 위/아래 두 개의 색상값을 가짐
- **DirectionalLight**: 방향성 광원 (그림자 생성 가능)
- **PointLight**: 점 광원 (그림자 생성 가능)
- **SpotLight**: 스포트라이트 (그림자 생성 가능)

> DirectionalLight, PointLight, SpotLight는 그림자를 생성할 수 있는 광원이다.

## 관련 문서

- [[JavaScript]] - Three.js의 기반 언어
- [[Computer-Graphics]] - 컴퓨터 그래픽스 기초 개념
- [[WebGL2]] - Three.js가 기반하는 웹 그래픽스 API
