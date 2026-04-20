---
title: Signed Distance Function (SDF)
type: concept
created: 2026-04-20
updated: 2026-04-20
sources:
  - sources/level-set-topology-optimization.pdf
tags:
  - SDF
  - implicit-surface
  - level-set
  - computer-graphics
---

# Signed Distance Function (SDF)

공간의 한 점에서 어떤 경계 ∂Ω까지의 **부호가 있는 거리** 를 반환하는 스칼라 함수. Level Set Method에서 ψ를 유지하는 표준 형태이며 VFX·CAD·컴퓨터 그래픽스에서도 광범위하게 쓰인다.

## 정의

$$\psi(x) = \pm\,\text{dist}(x, \partial\Omega), \quad |\nabla\psi| = 1$$

부호 규약:

- ψ(x) < 0 → 내부 (Ω)
- ψ(x) = 0 → 경계 (∂Ω)
- ψ(x) > 0 → 외부

**절댓값은 경계까지의 유클리드 거리** 를 의미한다. 경사의 크기는 공간 어디에서든 1 (Eikonal 조건).

## 왜 SDF를 쓰는가

- **수치 안정성**: 경사 $|\nabla\psi|=1$이 공간 전체에서 균일해 이산화 오차가 작음
- **법선이 간단**: $\hat{n} = \nabla\psi$ (정규화 불필요)
- **경계까지 거리가 즉시 얻어짐**: narrow-band 기법에서 중요 (경계에서 k칸 이내만 계산)
- **기하 연산이 간결**: 두 SDF의 min/max로 합집합/교집합 구현
- **렌더링 친화적**: Sphere Tracing(ray marching)에서 한 번의 거리 평가로 안전 진행 가능

## 재초기화 필요성

Hamilton-Jacobi 등 다른 PDE로 ψ를 진화시키면 일반적으로 $|\nabla\psi| \ne 1$이 된다. SDF 성질이 무너지면 이산화가 불안정해지므로 주기적으로 **재초기화(re-initialization)** 가 필요하다. 대표적 방법:

- **[[Eikonal-Equation]] PDE 풀기** (반복 해법)
- **Fast Marching Method (FMM)**: O(N log N), 우선순위 큐 기반
- **Fast Sweeping Method**: O(N), 축 정렬 스윕
- **Narrow-band 갱신 (OpenVDB)**: 경계 인근만 계산해 대규모 3D에서 효율적

## 이산 표현

| 방식 | 특징 | 용도 |
|---|---|---|
| **Dense grid** | 전체 공간 격자에 값 저장 | 작은 도메인, 단순 |
| **Narrow-band** | 경계 인근 k칸만 저장 | 대규모 장면, 실시간 |
| **OpenVDB** | 희소 계층 트리 | VFX 표준, 대규모 3D |
| **Analytic SDF** | 수식(박스, 구, CSG 조합) | 프리미티브, sphere tracing 셰이더 |
| **Neural SDF** | MLP로 $\psi(x)$ 근사 | DeepSDF, NeRF 계열 |

## 주요 응용

- **[[Level-Set-Method]]**: 인터페이스 추적의 표준 표현
- **Ray Marching / Sphere Tracing**: Shadertoy, 프로시저럴 렌더링
- **Collision detection**: 로봇, 게임 물리
- **Font rendering**: MSDF (multi-channel SDF)로 해상도 독립 폰트
- **3D printing**: STL 출력 전 중간 표현
- **CAD constructive solid geometry**: SDF의 min/max로 Boolean 연산

## 관련 페이지

- [[Level-Set-Method]]
- [[Eikonal-Equation]]
- [[Hamilton-Jacobi-Equation]]
- [[Level-Set-Topology-Optimization]]
- [[Computer-Graphics]]
