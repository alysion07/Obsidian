---
title: Level Set 기반 위상 최적화 - Theory Note
type: summary
created: 2026-04-20
updated: 2026-04-20
sources:
  - sources/level-set-topology-optimization.pdf
  - sources/level-set-topology-optimization.docx
tags:
  - level-set
  - topology-optimization
  - numerical-method
  - FEM
  - structural-optimization
---

# Level Set 기반 위상 최적화 - Theory Note

논문 *"Implementation of Level Set based Topology Optimization"*을 바탕으로 구조 위상 최적화(structural topology optimization) 전 과정을 이론·수식·알고리즘·구현 이슈까지 정리한 Single Source of Truth 문서.

## 핵심 아이디어

형상 최적화(shape optimization)는 고정된 위상(topology)에서 경계만 움직일 수 있어 구멍이 새로 생기거나 합쳐지는 변화를 표현하지 못한다. Level Set Method는 도메인을 고차원 함수 ψ(x, t)의 **0-등위선(zero level-set)** 으로 암묵적(implicit)으로 표현하여 이 한계를 돌파한다.

- ψ(x) < 0 → 재료 내부 Ω
- ψ(x) > 0 → 외부 (void)
- ψ(x) = 0 → 경계 ∂Ω

ψ 자체는 연속적으로 진화하지만, 그 0-등위선이 끊기거나 합쳐지면서 위상 변화(topology change)를 자연스럽게 수용한다. 이것이 Level Set 기반 위상 최적화의 본질.

## 수학적 뼈대

### 1. Signed Distance Function (SDF)

수치 안정성을 위해 ψ를 대개 SDF로 초기화한다:

$$\psi(x) = \pm \text{dist}(x, \partial\Omega), \quad |\nabla\psi| = 1$$

- ψ의 절대값이 경계까지의 거리를 의미한다
- 경사가 균일(|∇ψ|=1)해 수치 계산이 안정적이다
- 최적화 진행 중 ψ가 왜곡되면 Eikonal 방정식으로 **재초기화(re-initialization)** 한다

### 2. Hamilton-Jacobi Evolution Equation

경계의 재료 도함수(material derivative)가 0이라는 조건에서 유도된 ψ의 진화 PDE:

$$\frac{\partial\psi}{\partial t} = -v|\nabla\psi| - \omega g$$

| 항 | 의미 | 역할 |
|---|---|---|
| $-v\|\nabla\psi\|$ | Shape Sensitivity | 기존 경계를 법선 방향으로 이동(경계 변형·축소·확장) |
| $-\omega g$ | Topology / Nucleation | 도메인 내부 새로운 구멍 생성(핵생성 항) |
| $\partial\psi/\partial t$ | 시간 도함수 | 한 스텝 동안의 ψ 갱신량 |

### 3. Sensitivity

- **Shape sensitivity**: $v(x) = \sigma(u):\varepsilon(u) - \lambda$ (컴플라이언스 최소화 + 부피 제약 Lagrange multiplier λ)
  - v가 크면 "변형 에너지 큰 자리" → 유지해야
  - v가 음이면 "자원 낭비" → 밀어내기(구멍 확장)
- **Topology sensitivity g(x)**: 도메인 내부에 새 구멍을 뚫었을 때 목적함수 변화율 (topology derivative)
  - 예제 §8에서는 ω=0으로 두어 topology term을 비활성화하고 shape derivative만으로 진화

### 4. Eikonal Equation (Re-initialization)

Hamilton-Jacobi 진화 후 ψ는 더 이상 |∇ψ|=1이 아니다. 이를 SDF로 되돌리는 PDE:

$$\frac{\partial\psi}{\partial t} + w \cdot \nabla\psi = S(\psi_0), \quad w = S(\psi_0) \cdot \nabla\psi / |\nabla\psi|$$

- S(ψ₀): 초기 ψ 부호에 따라 결정되는 smoothed sign function
- 수렴하면 |∇ψ|=1인 SDF 복원
- 실무에서는 **Fast Marching Method (FMM)** 이나 **Fast Sweeping Method**를 주로 사용 (narrow-band/OpenVDB 활용)

## 전체 알고리즘

```
① Mesh the Domain
② Initialize Level Set Function (SDF ψ₀)
③ Map Physical Properties with LSF values (ψ>0은 ersatz/void 10⁻³·E_solid)
④ Solve the Physics (KU = F)
⑤ Calculate Shape & Topology Sensitivity
⑥ Evolve the LSF (Hamilton-Jacobi step, upwind + TVD-RK)
⑦ Converged? → Yes: Stop
              → No: ⑧ Re-initialize LSF (Eikonal/FMM) → back to ④
```

### 수치 팁 (문서 §7)

- ψ 이류는 보통 1st-order upwind + TVD-RK(2~3차) Runge-Kutta
- CFL 조건: Δt ≤ Δx / max|v|
- 부피 제약 λ는 bisection 또는 augmented Lagrangian로 보정
- 재초기화는 5~10 스텝마다 1회
- ersatz 재료 E_void = 10⁻³·E_solid (수치 특이성 회피)

## 예제 — 캔틸레버 보 (§8)

| 항목 | 값 |
|---|---|
| Design domain | W=2, H=1 |
| Mesh | 80 × 40 (3,200 Q4 요소) |
| Volume constraint λ | 100 |
| Iteration | 100회 (60~80회부터 수렴 시작) |
| ω | 0 (비활성화) |

ω=0이어도 shape term만으로 경계가 진화해 고전적인 Michell truss 유사 형태의 대각선 트러스 구조가 나타난다. Topology term(ω>0)을 켜면 SIMP 계열처럼 내부 구멍 생성이 더 활발하다.

## 주요 응용 분야 (§9)

| 분야 | Level Set 특화 이점 |
|---|---|
| 경량화·항공우주 | 매끈한 경계, 다공성 구조 자연 표현 |
| AM (3D 프린팅) | SDF → STL 변환 자연스러움 |
| 자연 형상화 | 성장(growth) 패턴 모사 |
| VFX·컴퓨터그래픽스 | OpenVDB narrow-band, FLIP·SPH 결합 |
| 바이오메디컬 | 장기/임플란트 형상 설계 |
| 가전·열 관리 | 유체·열 해석과 결합된 level set |

## 구현 시 주의점 (§10)

- 초기 level set 선택이 결과 형상에 영향 → topology derivative / SIMP 병행 / XFEM 고려
- 재초기화 빈도와 방법 선택 (narrow-band + OpenVDB)
- v 분포 처리, CFL 안정성, TVD-RK 고차 보정
- 수치 특이성: ersatz 재료, 완화 처리
- **GPU 가속**: WebGPU 등으로 실시간 인터랙티브 TO 가능

## 주요 참고문헌 (§12)

- **Wang, M.Y., Wang, X., Guo, M.** (2003) *A level set method for structural topology optimization.* CMAME 192 — 구조 최적화 분야의 이정표
- **Allaire, G., Jouve, F., Toader, A-M.** (2004) *Structural optimization using sensitivity analysis and a level-set method.* JCP 194
- **Osher, S., Sethian, J.** (1988) *Fronts propagating with curvature-dependent speed.* JCP 79 — Level Set의 기원
- **Sethian, J.** (1999) *Level Set Methods and Fast Marching Methods*, Cambridge UP — FMM 표준서
- **Osher, S., Fedkiw, R.** (2003) *Level Set Methods and Dynamic Implicit Surfaces*, Springer — VFX·공학 입문서
- **van der Laan, OpenVDB** — narrow-band SDF 데이터 구조 표준

## 관련 페이지

- [[Level-Set-Method]]
- [[Topology-Optimization]]
- [[Hamilton-Jacobi-Equation]]
- [[Signed-Distance-Function]]
- [[Eikonal-Equation]]
