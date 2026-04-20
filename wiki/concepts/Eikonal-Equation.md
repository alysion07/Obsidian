---
title: Eikonal Equation
type: concept
created: 2026-04-20
updated: 2026-04-20
sources:
  - sources/level-set-topology-optimization.pdf
tags:
  - PDE
  - eikonal
  - level-set
  - fast-marching
  - numerical-method
---

# Eikonal Equation

경사의 크기가 1인 정상상태(steady-state) 비선형 PDE:

$$|\nabla\psi(x)| = 1, \quad \psi = 0 \text{ on } \partial\Omega$$

해 ψ는 경계 ∂Ω에서부터의 **부호 없는 거리 함수**. 부호를 붙이면 [[Signed-Distance-Function]]이 된다. 기하광학(빛의 파면 전파), 지진파, 형상 처리 등에 쓰이며, Level Set Method에서는 **SDF 재초기화**의 핵심 도구.

## 유도 배경

Hamilton-Jacobi 진화가 진행되면 ψ가 "경계에서 멀어질수록 $|\nabla\psi|=1$" 성질을 잃는다. $|\nabla\psi|$가 0 근처면 수치 진화가 발산, 반대로 너무 크면 경계 이동 속도가 왜곡된다. 이를 막으려면 ψ를 "다시 SDF로 만들어야" 한다. 그때 푸는 PDE가 Eikonal.

## 반복 해법 (Re-initialization PDE)

정상상태로 수렴하도록 가상 시간 τ에 대한 이류(advection) 형태로 푼다:

$$\frac{\partial\psi}{\partial\tau} + w \cdot \nabla\psi = S(\psi_0), \quad w = S(\psi_0) \cdot \frac{\nabla\psi}{|\nabla\psi|}$$

- $S(\psi_0)$: 초기 ψ 부호에 따라 결정되는 smoothed sign function (진행 방향 결정)
- $w$: 초기 부호에 따라 경계 바깥으로 정보를 퍼뜨리는 속도장
- 수렴하면 $|\nabla\psi|=1$ 회복

이 방식은 간단하지만 반복 비용이 크고 경계 근처에서 numerical drift 가능성 있음.

## 직접 해법 알고리즘

### Fast Marching Method (FMM) - Sethian 1996

- **O(N log N)**, 우선순위 큐 기반
- 파면을 경계에서부터 "한 번에 한 점씩" 확장 (Dijkstra 유사)
- narrow-band에 최적, 실무에서 가장 널리 쓰임
- Sethian의 표준서: *Level Set Methods and Fast Marching Methods* (1999)

### Fast Sweeping Method - Zhao 2005

- **O(N)**, 축 정렬 방향으로 스윕
- 4방향(2D) 또는 8방향(3D) 반복 스윕
- FMM보다 단순, 균일 격자에 유리

## Level Set TO에서의 역할

논문(§6)에서 강조:

> Hamilton-Jacobi 한 스텝 후 ψ가 SDF 성질을 잃으면 다음 스텝의 $|\nabla\psi|$가 왜곡되어 속도 v가 제대로 반영되지 않는다. 그러므로 **5~10 스텝마다 재초기화**해야 한다.

재초기화 빈도는 설계 변수:
- 너무 자주 → 계산 비용 급증, 경계 이동이 둔해짐
- 너무 드물게 → 수치 불안정, 목적함수 발산

## 다른 Eikonal 응용

- **지진파 tomography**: 지진파 주행시간 계산
- **로봇 경로 계획**: 최단 경로 (distance field)
- **이미지 segmentation**: Geodesic active contours
- **VFX 유체**: OpenVDB narrow-band 유지 (FLIP/SPH과 결합)

## 관련 페이지

- [[Level-Set-Method]]
- [[Signed-Distance-Function]]
- [[Hamilton-Jacobi-Equation]]
- [[Level-Set-Topology-Optimization]]
