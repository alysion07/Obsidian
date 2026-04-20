---
title: Hamilton-Jacobi Equation
type: concept
created: 2026-04-20
updated: 2026-04-20
sources:
  - sources/level-set-topology-optimization.pdf
tags:
  - PDE
  - hamilton-jacobi
  - level-set
  - numerical-method
---

# Hamilton-Jacobi Equation

Level Set 함수 ψ(x, t)의 시간 진화를 지배하는 1차 비선형 편미분방정식(PDE). 경계가 속도장 v로 움직일 때 zero level-set이 해당 경계를 따라가게 만드는 방정식.

## 유도 (재료 도함수)

경계 ∂Ω 위의 한 점이 속도 v(x)로 움직일 때, 그 점에서 항상 ψ = 0이 유지되려면 ψ의 재료 도함수(material derivative)가 0이어야 한다:

$$\frac{d\psi}{dt} = \frac{\partial\psi}{\partial t} + (\nabla\psi) \cdot \frac{dx}{dt} = 0$$

경계 법선 $\nabla\psi/|\nabla\psi|$ 방향으로 v만큼 움직이면 $dx/dt = v \cdot \nabla\psi/|\nabla\psi|$, 따라서 $(\nabla\psi) \cdot (dx/dt) = v|\nabla\psi|$. 여기에 위상 최적화의 **핵생성(nucleation)** 항 $-\omega g$를 추가하면 최종 형태:

$$\boxed{\frac{\partial\psi}{\partial t} = -v|\nabla\psi| - \omega g}$$

## 각 항의 의미

| 항 | 이름 | 역할 |
|---|---|---|
| $-v\|\nabla\psi\|$ | Shape Sensitivity | 기존 경계를 법선 방향으로 이동(경계 변형·축소·확장) |
| $-\omega g$ | Topology / Nucleation | 도메인 **내부**에 새로운 구멍 생성(핵생성 항) |
| $\partial\psi/\partial t$ | 시간 도함수 | 한 스텝 동안의 ψ 갱신량 |

## 왜 |∇ψ|가 곱해지는가

속도 v는 "경계의 법선 방향 속도"이지만, 실제로 업데이트하는 것은 ψ 자체. 경계에서 멀어질수록 $\nabla\psi$가 크거나 작을 수 있으므로 속도를 ψ값 변화로 환산할 때 $|\nabla\psi|$로 스케일해야 한다. SDF로 유지되면 $|\nabla\psi|=1$이므로 이 인자가 단순해진다.

## 수치 이산화

### 시간 적분

- **1st-order Euler**: $\psi^{n+1} = \psi^n - \Delta t \cdot v|\nabla\psi^n|$ (간단하지만 저차)
- **TVD Runge-Kutta 2/3차**: 진동 억제, 안정성 향상 (실무 권장)

### 공간 이산화 - Upwind Scheme

$v|\nabla\psi|$는 비선형 쌍곡 항이므로 단순 중심차분이 불안정. **Upwind scheme**으로 안정화:

- v > 0: 좌측 차분 $\nabla\psi^-$ 사용 (정보가 오른쪽으로 흐름)
- v < 0: 우측 차분 $\nabla\psi^+$ 사용

보통 1st-order upwind + TVD-RK 조합이 표준.

### CFL 안정 조건

$$\Delta t \le \frac{\Delta x}{\max|v|}$$

속도가 크면 작은 timestep 필요. shape sensitivity v의 최대값에 맞춰 Δt 결정.

## 재초기화와 결합

진화를 진행할수록 ψ가 SDF 성질($|\nabla\psi|=1$)에서 멀어진다 → 5~10 스텝마다 [[Eikonal-Equation]]으로 재초기화해야 수치 안정성 유지.

## 관련 페이지

- [[Level-Set-Method]]
- [[Level-Set-Topology-Optimization]]
- [[Eikonal-Equation]]
- [[Signed-Distance-Function]]
