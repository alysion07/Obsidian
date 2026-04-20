---
title: "Photogrammetry (사진측량)"
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-PIX4D-AREA.md
tags:
  - 사진측량
  - 3D복원
  - GCP
  - 포인트클라우드
---

# Photogrammetry (사진측량)

2D 이미지로부터 3D 정보를 추출하는 기술이다.

## 주요 용어 (PIX4D Glossary)

- **Oblique**: 경사 촬영 이미지
- **Nadir**: 천저 이미지 (90도 수직 촬영)
- **GCP (Ground Control Points)**: RTK/PPK GNSS로 측정된 정확한 좌표점
- **MTP (Manual Tie Points)**: 사용자가 이미지에서 표시한 3D 점
- **CheckPoint**: 모델의 절대 정확도 평가용 포인트
- **Point Cloud Library (PCL)**: 포인트 클라우드 처리 라이브러리
- **Point Cloud Tileset (pnts)**: 포인트 클라우드 타일셋 포맷

## 처리 파이프라인

```
Calibration -> Dense Point Cloud -> Mesh -> Texture -> Ortho/DSM/DTM
```

## 처리 유형

- **CATCH**: 근접 촬영 기반 3D 모델링
- **NADIR**: 수직 촬영 기반 정사영상 생성
- **OBLIQUE**: 경사 촬영 기반 3D 도시 모델링

## 정확도

- X, Y: 5cm tolerance
- Z: 15cm tolerance

## Related

- [[PIX4D]]
- [[Gaussian-Splatting]]
