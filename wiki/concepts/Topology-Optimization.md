---
title: Topology Optimization (위상 최적화)
type: concept
created: 2026-04-20
updated: 2026-04-20
sources:
  - sources/level-set-topology-optimization.pdf
tags:
  - topology-optimization
  - structural-optimization
  - FEM
  - design
---

# Topology Optimization (위상 최적화)

주어진 설계 영역 D와 하중·경계조건·부피 제약 하에서 **재료의 분포(어디에 재료가 있고 어디가 비어 있어야 하는가)** 를 결정하여 목적함수(보통 컴플라이언스)를 최소화하는 최적화 기법.

## 3가지 최적화 수준

| 수준 | 바꾸는 것 | 예 |
|---|---|---|
| **Size optimization** | 단면적·두께 등 파라미터 | 트러스 부재 굵기 |
| **Shape optimization** | 경계 형상 (고정 위상) | 고정된 구멍 개수에서 경계만 변형 |
| **Topology optimization** | 재료 분포 자체 (구멍 개수 포함) | 재료가 있을지 없을지, 구멍 몇 개인지까지 |

위상 최적화가 가장 자유도가 크며, 경량화 설계의 출발점이 된다.

## 문제 정형화 (Compliance Minimization)

$$\min_{\psi} \quad J(\psi) = \tfrac{1}{2} U^T K(\psi) U$$
$$\text{s.t.} \quad K(\psi) U = F, \quad \int_{\{\psi \le 0\}} dx \le V^*$$

- $K(\psi)$ = 현재 재료 분포에 대응하는 전역 강성 행렬
- $J$ = 컴플라이언스 (변형 에너지 × 2)
- $V^*$ = 허용 부피 상한

## 주요 접근법

| 방법 | 표현 방식 | 특징 |
|---|---|---|
| **SIMP** (Solid Isotropic Material with Penalization) | 각 셀에 밀도 ρ∈[0,1] | 가장 대중적, MATLAB 88-line 코드로 유명 |
| **Level Set Method** | 경계를 $\psi=0$ 등위선으로 암묵 표현 | 매끈한 경계, 자연스러운 위상 변화 ([[Level-Set-Topology-Optimization]]) |
| **Homogenization** | 미세구조 파라미터 | 이론적으로 원조(Bendsøe-Kikuchi 1988) |
| **ESO/BESO** | 요소 추가/제거 | 직관적, 경사 없이도 동작 |
| **XFEM + Level Set** | 경계를 요소 내부에서 처리 | 셰이프 함수 확장으로 인터페이스 정확도↑ |

## 민감도(Sensitivity)

목적함수 $J(\Omega)$가 형상 변화(속도장 $v$)에 대해 미소 섭동 $\varepsilon v$만큼 변할 때의 도함수:

- **Shape sensitivity** $v(x) = \sigma(u):\varepsilon(u) - \lambda$
  - 컴플라이언스 최소화 + 부피 제약 λ (Lagrange multiplier)
  - $\sigma:\varepsilon$ = 국소 변형 에너지 밀도
- **Topology sensitivity (topological derivative)** $g(x)$
  - 도메인 내부에 무한소 구멍을 뚫었을 때 목적함수 변화율
  - $g < 0$이면 그 위치에 구멍을 뚫으면 개선 → 새 구멍 생성(nucleation)

## 대표 응용

- **항공우주 / 자동차 경량화**: 기체 프레임·서스펜션 암 무게 절감
- **AM (3D 프린팅)**: 자유 곡면 제조 가능해지면서 TO 결과를 그대로 제조
- **바이오메디컬**: 맞춤형 임플란트, 뼈와 유사한 격자 구조
- **MEMS**: 마이크로 구조 설계
- **건축 구조**: Michell truss 같은 고전 해에 수렴하는 자유 설계

## 관련 페이지

- [[Level-Set-Method]]
- [[Level-Set-Topology-Optimization]] — Level Set 기반 구현 요약
- [[Hamilton-Jacobi-Equation]]
- [[Signed-Distance-Function]]
