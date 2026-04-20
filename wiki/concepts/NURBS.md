---
title: NURBS
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 3D모델링
  - CAD
  - 곡면
---

# NURBS (Non-Uniform Rational B-Splines)

NURBS는 3D 형상을 수학적으로 표현하는 데 적합한 곡면 모델링 기법이다. Polygon Model은 평활도가 부족하기 때문에 최종 설계에는 NURBS가 사용된다.

## NURBS가 사용되는 이유

- **Flexibility**: 조각적인(sculptural) 형상을 자유롭게 생성 가능
- **Tension**: 표면을 부드럽고 팽팽하게 유지
- **Alignment**: 부드럽고 눈에 보이지 않는 접합부 생성

## NURBS vs Polygon 비교

### Polygon 모델링의 장점
- 표면 디테일 만들기 (캐릭터 얼굴의 주름, 초목, 바위 등)
- 빠른 렌더링 계산 (이미 테셀레이션되어 있음)

### Polygon 모델링의 한계
다각형 모델은 부드러운 모양에 가까운 평평한 패싯(Facet)의 집합이다. 렌더링이나 프로토타이핑에는 적합하지만(NURBS 모델도 이러한 목적을 위해 다각형으로 변환됨), **프로덕션에는 적합하지 않다.**

### NURBS의 강점
최종 제품용 툴링을 만드는 CNC 공작 기계는 정확하고 부드러운 **NURBS 데이터**를 기반으로 작동한다. 수학적으로 정밀한 곡면 표현이 가능하여 CAD/CAM 분야에서 필수적이다.

## 관련 문서

- [[Computer-Graphics]] - 컴퓨터 그래픽스 전반
