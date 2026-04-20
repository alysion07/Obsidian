---
title: Deferred Rendering
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 그래픽스
  - 렌더링
  - 최적화
---

# Deferred Rendering

Deferred Rendering(지연 렌더링)은 기존 Forward Rendering의 성능 한계를 극복하기 위해 고안된 렌더링 기법이다. 조명 계산을 기하 렌더링 단계에서 분리하여 후처리 단계에서 수행한다.

---

## Forward Rendering의 한계

Forward Rendering은 Scene에 있는 모든 광원에 따라 각 객체를 개별적으로 렌더링하고 조명을 적용하는 방식이다.

- 이해하고 구현하기는 쉬움
- 객체 수 x 광원 수만큼 조명 계산이 필요하여, 광원이 많아지면 성능이 급격히 저하
- 보이지 않는 Fragment에 대해서도 조명 계산이 수행될 수 있음

---

## Deferred Rendering 원리

Deferred Rendering은 렌더링을 두 단계로 나눈다.

### 1단계: Geometry Pass (G-Buffer 생성)

첫 번째 패스에서는 모든 기하 정보를 G-Buffer(Geometry Buffer)에 기록한다. 조명 계산은 수행하지 않는다.

G-Buffer에 저장되는 주요 정보:
- **Position** - 각 픽셀의 월드 좌표 위치
- **Normal** - 각 픽셀의 법선 벡터
- **Albedo/Diffuse Color** - 각 픽셀의 기본 색상
- **Specular** - 반사 강도 및 특성
- **Depth** - 깊이 값

이 정보들은 별도의 텍스처(렌더 타겟)에 각각 기록된다. WebGL2의 `out` 키워드를 사용하면 Fragment Shader가 여러 버퍼로 동시에 출력할 수 있다 (MRT: Multiple Render Targets).

### 2단계: Lighting Pass (조명 계산)

두 번째 패스에서는 G-Buffer에 저장된 정보를 읽어 각 픽셀에 대한 조명 계산을 수행한다. 화면 전체를 덮는 쿼드(full-screen quad)를 그리면서 각 픽셀에 대해 G-Buffer의 데이터를 참조하여 최종 색상을 계산한다.

---

## 장점

- **광원 수에 대한 성능 스케일링이 우수**: 조명 계산이 화면의 픽셀 수에만 비례하므로, 광원이 많아져도 성능 저하가 적다
- **불필요한 조명 계산 제거**: 가려진 Fragment에 대한 조명 계산이 발생하지 않음 (이미 Depth Test를 통과한 픽셀만 G-Buffer에 기록됨)
- **후처리 효과 구현 용이**: G-Buffer에 필요한 정보가 모두 있으므로 SSAO, Bloom 등의 후처리 효과 적용이 수월

---

## 단점

- **메모리 사용량 증가**: G-Buffer를 위한 여러 장의 텍스처가 필요
- **투명 객체 처리 어려움**: G-Buffer에는 한 픽셀당 하나의 기하 정보만 저장할 수 있어, 투명 객체는 별도의 Forward Rendering 패스가 필요
- **안티앨리어싱 제한**: 기존 MSAA 적용이 어렵고, 후처리 기반 AA(FXAA, TAA 등)를 사용해야 함
- **대역폭 요구**: G-Buffer 읽기/쓰기에 높은 메모리 대역폭이 필요

---

## 조명 계산과 벡터 내적

Deferred Rendering의 Lighting Pass에서 가장 기본이 되는 조명 모델은 Diffuse Shading(램버트 코사인 법칙)이다.

```glsl
vec3 normal = normalize(v_normal);
float light = dot(normal, u_reverseLightDirection);
outColor.rgb *= light;
```

표면의 법선 벡터(N)와 빛의 방향 벡터(L)의 내적(N dot L)으로 빠르게 명암을 계산한다. 오브젝트에 스케일이 적용된 경우, normal에 world matrix의 inverse transpose 행렬을 적용해야 정확한 결과를 얻을 수 있다.

---

## Related Pages

- [[Computer-Graphics]] - 컴퓨터 그래픽스 종합 개념
- [[RS-Engine]] - RS-Engine 프로젝트 (Deferred Rendering 적용)
- [[WebGL2]] - WebGL2 학습 시리즈
