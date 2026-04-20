---
title: RS Engine
type: entity
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-ARCHIVE-E8.md
tags:
  - 그래픽스
  - 렌더링
  - 엔진
  - E8
---

# RS Engine

## 개요

E8ight GraphicsDev 팀에서 개발 중인 렌더링/시각화 엔진. SPH Viewer 등의 프로젝트에서 활용.

## 개발 기간

2025-02-24 ~ 2026-01-02

## 핵심 기술 개발 항목

### Engine Core
- Material / Depth Buffer / [[Deferred-Rendering]] 병행 개발
- 4월 말 마지노선 목표

### 시각화 기능
- **Stream line** (유적선): Geometry Shader 기반, 2개월
- **Vector Visualization**: 1개월
- **Contour** (Legend 단층화): 2주
- **ISOsurface** (Data Clip): 2주

### 물 효과
- [[Deferred-Rendering]], Depth Buffer, SSF, SSAO, Normal Map
- Algorithm: Marching Cube, Ray Marching
- Shader: Refraction, Reflect, Fresnel, Blur, Foam/Bubbles

### Material System (2개월)
- PBR (Physically Based Rendering)
- Multi Texturing (Normal Map 유무 추가)

## 관련

- [[NFLOW]]
- [[Deferred-Rendering]]
