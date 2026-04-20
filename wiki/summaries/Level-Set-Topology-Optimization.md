---
title: Level Set 기반 위상 최적화 (Theory Note)
type: summary
created: 2026-04-20
updated: 2026-04-20
sources:
  - sources/level-set-topology-optimization.pdf
  - sources/level-set-topology-optimization.docx
tags:
  - level-set
  - topology-optimization
  - structural-optimization
  - hamilton-jacobi
  - eikonal
  - SDF
  - theory-note
---

# Level Set 기반 위상 최적화 — 이론 노트 요약

**원문**: `sources/level-set-topology-optimization.pdf` (Level Set Method for Structural Topology Optimization · Single Source of Truth · 2026)

구조 위상 최적화(structural topology optimization)를 Level Set 표현으로 풀기 위한 이론·수식·알고리즘·응용을 한 문서에 정리한 Theory Note. 각 장은 한 가지 "왜"와 그에 대응하는 "어떻게"로 쌍을 이룬다.

## 1. 문제 정의 — 왜 Level Set인가

구조 최적화의 본질 질문은 *"재료가 어디에 있고 어디가 비어야 하는가"* 이며, 이는 **경계(boundary)의 문제**로 환원된다. 경계를 파라미터 곡선으로 쫓으면 위상(구멍 개수)이 바뀔 때 파라미터화가 무너진다. Level Set Method는 경계를 고차원 함수의 **등위선**으로 암묵적으로 정의하여 이 문제를 회피한다.

## 2. 수학적 표현

Level Set 함수 ψ(x, t)로 도메인 Ω와 외부를 구분:

| 부호 | 의미 |
|---|---|
| ψ(x) < 0 | 내부 (재료 있음) Ω |
| ψ(x) = 0 | 경계 ∂Ω |
| ψ(x) > 0 | 외부 (void) |

경계가 둘로 쪼개지거나 합쳐져도 ψ는 연속 함수로 유지된다 → **위상 변화(topology change)가 자연스럽게 일어난다**. 자세한 논의: [[Level-Set-Method]].

## 3. SDF와 수치 안정성

수치 해법에서는 ψ를 [[Signed-Distance-Function]](SDF)로 유지한다:

$$\psi(x) = \pm\,\text{dist}(x, \partial\Omega), \quad |\nabla\psi| = 1$$

이산화 오차가 균일해지고 법선·곡률 계산이 단순해진다. 그러나 HJ 진화 과정에서 이 성질이 깨지므로 주기적 재초기화 필요.

## 4. Hamilton-Jacobi 진화 방정식

경계가 속도장 v로 움직일 때 ψ는 아래 PDE를 따른다:

$$\boxed{\,\frac{\partial\psi}{\partial t} = -v|\nabla\psi| - \omega\,g\,}$$

| 항 | 역할 |
|---|---|
| $-v\|\nabla\psi\|$ | Shape sensitivity — 기존 경계를 법선 방향으로 이동 |
| $-\omega g$ | Topology/nucleation — 도메인 **내부**에 새 구멍 생성 |

유도는 재료 도함수 $d\psi/dt=0$에서 출발. 자세히: [[Hamilton-Jacobi-Equation]].

## 5. 민감도 — 경계 속도의 결정

### 5.1 Shape Sensitivity v(x)

컴플라이언스 최소화 + 부피 제약에서:

$$v(x) = \sigma(u):\varepsilon(u) - \lambda$$

- $\sigma:\varepsilon$ = 국소 변형 에너지 밀도 (크면 재료 필요, 경계 팽창)
- $\lambda$ = 부피 제약에 대한 Lagrange multiplier (예제 λ=100)

### 5.2 Topology Sensitivity g(x)

도메인 내부에 무한소 구멍을 뚫었을 때 목적함수 변화율. $g(x)<0$이면 그 위치에 구멍 생성이 개선 → **핵생성(nucleation)**.

> **기본 Level Set의 한계**: 순수 shape sensitivity만 쓰면(ω=0) *내부에 새 구멍을 만들지 못한다*. 초기 구멍 배치에 결과가 의존하는 것은 이 때문. 극복 방법: topology derivative 결합, SIMP 혼합, XFEM 등.

## 6. Eikonal 재초기화

HJ 한 스텝 후 $|\nabla\psi|\ne 1$로 왜곡되면 다음 스텝의 속도 반영이 틀어진다. 주기적(5~10 스텝)으로 SDF를 복원:

$$\frac{\partial\psi}{\partial\tau} + w\cdot\nabla\psi = S(\psi_0),\quad w=S(\psi_0)\frac{\nabla\psi}{|\nabla\psi|}$$

- 반복 PDE 해법
- **Fast Marching Method (FMM)** — O(N log N), Dijkstra 유사, narrow-band에 최적
- **Fast Sweeping Method** — O(N), 축 정렬 스윕

자세히: [[Eikonal-Equation]].

## 7. 전체 알고리즘 플로우

```
① Mesh the Domain
② Initialize Level Set Function  (SDF ψ₀)
③ Map Physical Properties with LSF  (ersatz: E_void = 10⁻³·E_solid)
④ Solve KU = F                     (FEM, Q4 요소 등)
⑤ Calculate Shape & Topology Sensitivity  (v, g)
⑥ Evolve LSF (Hamilton-Jacobi, upwind + TVD-RK)
⑦ Converged?  — No → ⑧, Yes → Stop
⑧ Re-initialize LSF (Eikonal, 5~10 스텝마다) → ④로
```

**실무 팁**

- 공간: 1st-order upwind, 시간: TVD-RK 2~3차
- CFL: $\Delta t \le \Delta x / \max|v|$ (shape sensitivity v의 최댓값 기준)
- 부피 제약 λ: bisection 또는 augmented Lagrangian
- 재초기화: 5~10 스텝마다 (더 잦으면 이동 느림, 더 드물면 불안정)
- Ersatz 재료: 빈 도메인에 $E_{\text{void}} = 10^{-3}\,E_{\text{solid}}$로 수치 특이성 회피

## 8. 예제 — MBB 보 (Michell truss)

고정 끝 왼쪽, 반대쪽 상단에 하중 F가 걸리는 대표 벤치마크.

| 파라미터 | 값 |
|---|---|
| Domain W × H | 2 × 1 |
| Mesh | 80 × 40 (3,200 Q4 요소) |
| Volume constraint λ | 100 |
| Iteration | 100 (수렴 보통 60~80) |
| ω (topology term) | 0 (shape만 사용) |

최적화 문제:

$$K(\psi)U=F,\quad \min_\psi J(\psi)=\tfrac{1}{2}U^\top K(\psi)U\quad\text{s.t.}\quad\int_{\{\psi\le 0\}}dx\le V^*$$

ω=0이므로 내부 구멍은 새로 생성되지 않고 초기 ψ₀의 구멍 배치에서 출발. 결과는 고전 **Michell truss** 해 근처로 수렴.

## 9. 응용 분야

| 분야 | 기존 방법 한계 | Level Set의 기여 |
|---|---|---|
| 항공우주·자동차 | 파라미터·토폴로지 동시 변경 어려움 | 자유 토폴로지 경량화, 매끈 경계 |
| AM (3D 프린팅) | STL·메시 변환 품질 | SDF → STL 변환이 매끈, 제조 직결 |
| 바이오 임플란트 | 개별 맞춤 설계 | 뼈 유사 격자·성장(growth) 설계 |
| VFX·컴퓨터그래픽스 | OpenVDB narrow-band, 유체 인터페이스 | FLIP/SPH과 결합, 인터페이스 추적 |
| 디지털트윈 | 손상 영역 표현 | 변형 field로 손상 경계 추적 |
| MEMS·건축 | 미세 구조·자유 형상 | 자유 곡면·격자 구조 설계 |

## 10. 실무에서의 고려사항

- **구멍 생성 문제** — 순수 level set은 내부 새 구멍 생성 불가. ω>0 (topology derivative), SIMP 결합, XFEM 이용.
- **재초기화 비용** — Eikonal 반복이 병목. Fast Marching / Fast Sweeping, narrow-band (OpenVDB)로 경감.
- **시간 적분** — v 크기에 따라 CFL, TVD-RK로 안정화.
- **수치 이산화** — upwind 필수, 중심차분은 쌍곡 항에 불안정.
- **GPU 가속** — narrow-band가 희소 구조라 GPU(WebGPU 포함)와 궁합 좋음.

## 11. 용어 요약

| 한글 | 영문 | 요약 |
|---|---|---|
| 레벨 셋 함수 | Level Set Function ψ | 0-등위선으로 경계를 암묵 표현 |
| 부호거리함수 | Signed Distance Function (SDF) | 부호 포함 경계까지의 거리 |
| 해밀턴-야코비 방정식 | Hamilton-Jacobi equation | ψ 시간 진화 지배 PDE |
| 아이코널 방정식 | Eikonal equation | $\|\nabla\psi\|=1$ 정상상태 PDE |
| 형상 민감도 | Shape sensitivity v | 경계 법선 속도 |
| 위상 민감도 | Topology sensitivity g | 내부 구멍 생성 민감도 |
| 재초기화 | Re-initialization | ψ를 SDF로 복원 |
| 컴플라이언스 | Compliance | 변형 에너지 × 2 (강성의 역지표) |
| 부피 제약 승수 | Lagrange multiplier λ | 부피 제약 라그랑주 승수 |
| ersatz 재료 | Ersatz (void) material | 빈 영역에 낮은 강성 할당 |
| CFL 조건 | CFL condition | $\Delta t \le \Delta x/|v|$ |
| Narrow-band | Narrow-band level set | 경계 인근만 저장·갱신 |
| FMM | Fast Marching Method | Eikonal O(N log N) 해법 |
| 위상 최적화 | Topology Optimization (TO) | 재료 "있음/없음" 분포 결정 |

## 12. 참고문헌

- **Wang, Wang, Guo (2003)** *A level set method for structural topology optimization.* CMAME 192 — Level Set TO 최초 제안
- **Allaire, Jouve, Toader (2004)** *Structural optimization using sensitivity analysis and a level-set method.* JCP 194 — Shape sensitivity 정식화
- **Osher, Sethian (1988)** *Fronts propagating with curvature-dependent speed.* JCP 79 — Level Set 원조 논문
- **Sethian (1999)** *Level Set Methods and Fast Marching Methods*, Cambridge UP
- **Osher, Fedkiw (2003)** *Level Set Methods and Dynamic Implicit Surfaces*, Springer — VFX·컴퓨터 그래픽스
- **van der Laan, OpenVDB** — narrow-band SDF 표준 구현, VFX·CAD

## 관련 페이지

- [[Level-Set-Method]] — 방법론 자체
- [[Topology-Optimization]] — 위상 최적화 일반
- [[Hamilton-Jacobi-Equation]] — 진화 PDE
- [[Eikonal-Equation]] — 재초기화 PDE
- [[Signed-Distance-Function]] — ψ의 표준 형태
