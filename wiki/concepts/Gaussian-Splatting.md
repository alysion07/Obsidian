---
title: Gaussian Splatting
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 3D복원
  - 렌더링
  - NeRF
---

# Gaussian Splatting

## Gaussian(가우시안)이란?

자연 현상을 묘사할 때, 종(bell) 모양의 함수 형태를 띄면 이를 **가우시안(Gaussian)** 이라 칭한다. 가우시안 분포(정규분포)는 데이터 분석 및 이미지 처리에서 핵심적으로 사용되는 수학적 개념이다.

## 가우시안 블러와 평균값 필터의 차이

- **평균값 필터**: 필터링 대상 위치에서 가까이 있는 픽셀과 멀리 있는 픽셀이 모두 같은 가중치를 사용하여 평균을 계산한다. 멀리 있는 픽셀의 영향을 많이 받을 수 있다.
- **가우시안 필터**: 멀리 있는 픽셀들의 영향이 적어, 보다 자연스러운 블러 효과를 만들어낸다. 바둑판 등의 패턴에서 선이 조금 더 선명하게 유지된다.

## 3D Gaussian Splatting

3D Gaussian Splatting은 SIGGRAPH 2023에서 발표된 기술로, [[NeRF]]와는 다른 접근 방식의 3D 렌더링 기법이다. NeRF가 신경망 기반으로 장면을 암묵적으로 표현하는 반면, Gaussian Splatting은 3D 공간에 수많은 가우시안 분포를 배치하여 장면을 명시적으로 표현한다.

- CUDA 기반 래스터라이저를 활용하여 실시간에 가까운 렌더링 속도를 달성
- 렌더링 속도와 품질 모두에서 기존 방법 대비 개선

## 관련 개념

- [[Computer-Graphics]] - 컴퓨터 그래픽스 전반
- [[Photogrammetry]] - 사진 측량 기반 3D 복원
- [[NeRF]] - Neural Radiance Fields, 신경망 기반 3D 렌더링

## 참고 자료

- [3D Gaussian Splatting 완벽 분석 (feat. CUDA Rasterizer)](https://velog.io/@gjghks950/3D-Gaussian-Splatting-%EC%99%84%EB%B2%BD-%EB%B6%84%EC%84%9D-feat.-CUDA-Rasterizer)
- [논문 리뷰: 3D Gaussian Splatting (SIGGRAPH 2023)](https://xoft.tistory.com/51)
