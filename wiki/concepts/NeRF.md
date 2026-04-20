---
title: NeRF (Neural Radiance Fields)
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 3D복원
  - 딥러닝
  - 렌더링
---

# NeRF (Neural Radiance Fields)

NeRF는 "Neural Radiance Fields"의 약자로, 3D 장면을 생성하고 렌더링하기 위한 신경망 기반의 컴퓨터 비전 기술이다. 실제 세계의 3D 장면을 모델링하고 새로운 시점에서 장면을 볼 수 있도록 하는 것이 주요 목표이다.

## 개요

물체나 사람을 360도 돌려가며 다양한 각도에서 관측하려면 2D 이미지 대신 3D 렌더링이 필요하다. 전통적으로는 컬러로 입혀진 메쉬, 포인트 클라우드, 복셀로 이루어진 3D 모델을 사용하여 구현하며, SFM(Structure From Motion) 기술로 실제 세계 객체를 3D 재구성할 수 있다.

NeRF는 객체의 3D 모델을 생성하는 것이 아니라, 객체를 바라보는 모든 시점의 장면을 생성하는 **Novel View Synthesis** 기술이다. 특정 객체나 장면을 여러 시점에서 관찰한 결과로부터 촬영하지 않은 각도의 뷰를 생성하여 3D 렌더링된 것처럼 보여준다.

## 아키텍처

NeRF의 아키텍처는 MLP(Multi-Layer Perceptron) 네트워크를 사용한다.

- **입력**: 3D 공간의 위치 정보(x, y, z)와 방향 정보(theta, phi)
- **출력**: 해당 좌표에서의 RGB 컬러 값과 밀도(density, 투명도의 역수)

이를 통해 NeRF는 특정 각도에서 특정 위치의 컬러 값과 투명도를 예측한다.

## 장점

- **연속적 공간 표현**: 뷰 이동이 자연스럽다
- **저장 공간 효율**: 3D 모델 정보를 사용하여 광선과 물체 간의 관계를 계산하여 렌더링하는 기존 방법과 비교하여 저장 공간 부담이 적다
- **현실적 렌더링**: 컬러 값뿐만 아니라 볼륨 밀도까지 추정하므로 더 현실적인 렌더링 결과를 제공한다

## 관련 개념

- **SFM (Structure From Motion)**: 2D 이미지에서 3D 구조를 복원하는 기술
- **MLP (Multi-Layer Perceptron)**: 다층 퍼셉트론 신경망

## 관련 문서

- [[Gaussian-Splatting]] - 가우시안 스플래팅, NeRF와 비교되는 3D 렌더링 기법
- [[Computer-Graphics]] - 컴퓨터 그래픽스 전반
