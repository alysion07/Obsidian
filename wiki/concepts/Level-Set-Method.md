---
title: Level Set Method
type: concept
created: 2026-04-20
updated: 2026-04-20
sources:
  - sources/level-set-topology-optimization.pdf
tags:
  - level-set
  - implicit-surface
  - numerical-method
  - computational-geometry
---

# Level Set Method

경계(interface)나 도메인을 **고차원 함수의 등위선**으로 암묵적으로 표현하여 곡선·곡면의 진화를 다루는 수치 기법. Stanley Osher와 James Sethian이 1988년 JCP 79 논문에서 제안.

## 핵심 아이디어

2D의 닫힌 곡선을 1D로 쫓아다니면서 표현하려 하면, 곡선이 끊기거나 합쳐질 때 파라미터화가 무너진다. 대신 곡선을 3D 함수 ψ(x, y)의 **등위선(level set)** 으로 정의하면:

- 곡선은 ψ(x, y, t) = 0 (zero level-set)
- 곡선 자체는 **암묵적으로 저장되지 않고** ψ의 부호로 구분된다

| ψ의 부호 | 의미 |
|---|---|
| ψ(x) < 0 | 내부 (Ω) |
| ψ(x) = 0 | 경계 (∂Ω) |
| ψ(x) > 0 | 외부 |

곡선이 둘로 쪼개지거나 합쳐져도 ψ는 연속 함수로 남는다 → **위상 변화(topology change)가 자연스럽게 일어난다.**

## 진화 방정식

경계를 속도장 v(x)로 움직이면 ψ는 Hamilton-Jacobi PDE를 따른다:

$$\frac{\partial\psi}{\partial t} + v|\nabla\psi| = 0$$

- v > 0이면 경계가 외부 법선 방향으로 팽창
- v < 0이면 축소

자세한 유도와 이항(ω g) 추가는 [[Hamilton-Jacobi-Equation]] 참조.

## 수치 안정성 - SDF

ψ를 임의의 함수로 두면 경사가 뾰족해져 수치 이산화가 깨진다. 대부분의 구현은 ψ를 [[Signed-Distance-Function]](SDF)로 유지한다:

$$\psi(x) = \pm\text{dist}(x, \partial\Omega), \quad |\nabla\psi| = 1$$

SDF는 계산 진행 중 왜곡되므로 주기적으로 [[Eikonal-Equation]] 재초기화 필요.

## 주요 응용

- **Structural Topology Optimization**: 구조 위상 최적화 ([[Topology-Optimization]], [[Level-Set-Topology-Optimization]])
- **VFX / Computer Graphics**: 유체 시뮬레이션, 표면 추적 (OpenVDB narrow-band)
- **Image Segmentation**: 의료 영상에서 장기 경계 추적
- **Multiphase Flow**: 두 유체의 인터페이스 추적
- **Additive Manufacturing**: SDF → STL 변환이 매끈해 3D 프린팅과 궁합 좋음

## 장단점

| 장점 | 단점 |
|---|---|
| 위상 변화 자동 처리 (구멍 생성/합병) | 고차원 함수 저장 비용 (N → N+1 차원) |
| 매끈한 경계 표현 | 재초기화가 주기적으로 필요 |
| 법선/곡률 계산 간단 ($\nabla\psi/\|\nabla\psi\|$) | CFL 안정성 제약 |
| 노이즈에 강함 | 초기 ψ 선택에 결과 의존 |

## 핵심 참고문헌

- Osher & Sethian (1988) *Fronts propagating with curvature-dependent speed*, JCP 79 — 원조 논문
- Sethian (1999) *Level Set Methods and Fast Marching Methods*, Cambridge UP
- Osher & Fedkiw (2003) *Level Set Methods and Dynamic Implicit Surfaces*, Springer

## 관련 페이지

- [[Signed-Distance-Function]]
- [[Hamilton-Jacobi-Equation]]
- [[Eikonal-Equation]]
- [[Topology-Optimization]]
- [[Level-Set-Topology-Optimization]]
- [[Computer-Graphics]]
