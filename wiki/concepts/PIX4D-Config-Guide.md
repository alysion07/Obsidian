---
title: PIX4D Engine Config Guide
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-PIX4D-AREA.md
tags:
  - PIX4D
  - 설정
  - API
---

# PIX4D Engine Config Guide

PIX4D Engine SDK의 config.json 구조를 정리한 가이드이다.

## config.json 구조

### project settings

- `version`: 설정 파일 버전
- `project_name`: 프로젝트 이름
- `processing`: 처리 유형 (CATCH / NADIR / OBLIQUE)

### hw_resources

- `use_gpu`: GPU 사용 여부 (AWS 환경에서는 `false`)
- `max_ram_ratio`: 최대 RAM 사용 비율

### projection

- `horizontal_epsg`: 수평 좌표계 EPSG 코드
- `vertical_epsg`: 수직 좌표계 EPSG 코드
- `geoid`: 지오이드 모델

### calibration_settings

- `templates`: LARGE / FLAT / MAPS_3D / MODELS_3D / CATCH
- `image_scale`: 이미지 스케일
- `matching_algorithm`: 매칭 알고리즘
- `pipeline`: 처리 파이프라인

### dense_settings

- `image_scale`: 이미지 스케일
- `point_density`: 포인트 밀도
- `min_no_match`: 최소 매칭 수
- `window_size`: 윈도우 크기

### mesh_settings

- `templates`: LARGE / SMALL / TOWER

### ortho_settings

- `pipeline`: FAST / FULL / DEGHOST
- `capture_pattern`: 촬영 패턴

## 출력 포맷

- **GeoTIFF**: COG(Cloud Optimized GeoTIFF) 지원
- **LAS/LAZ**: 포인트 클라우드 포맷

## Related

- [[PIX4D]]
- [[Photogrammetry]]
