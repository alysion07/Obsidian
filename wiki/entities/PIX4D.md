---
title: PIX4D
type: entity
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-PIX4D-AREA.md
tags:
  - 사진측량
  - 드론
  - 3D
  - 포인트클라우드
---

# PIX4D

PIX4D는 사진측량(Photogrammetry) 도구이다. 단순한 드론 솔루션이 아니라, 2D 이미지로부터 3D 데이터를 생성하는 사진측량 전문 플랫폼이다.

## 제품군

- **PIX4D Capture**: 드론 비행 경로 설정 앱
- **PIX4D Survey**: 지형 그리드/TIN 내보내기
- **PIX4D Matic**: 대규모 사진측량 처리
- **PIX4D Cloud**: 클라우드 기반 처리 플랫폼
- **PIX4D Engine SDK**: 프로그래밍 가능한 사진측량 엔진

## PGP (Processed GigaPixel)

PGP는 PIX4D의 처리량 단위로, 1 PGP = 1000 MegaPixels이다.

- 예시: 200장 이미지 * 20MP = 4,000MP = **4 PGP**
- PGP는 **Calibration 단계**에서 소비된다. 이후 처리(Ortho, DSM, DTM, Mesh)에서는 추가 PGP가 소비되지 않는다.

### Cloud PGP vs SDK PGP

- Cloud PGP는 SDK PGP 대비 **1.5~3배 비싸다** (AWS 컴퓨팅 비용 포함)

## NDX Cloud Pix4D 플랫폼

- B2B PGP 분배 플랫폼
- API 기반 이미지 처리 -> PCD(Point Cloud Data) 데이터 생성

## BuildView

- PIX4D Cloud API 기반 MVP
- 초대 기반 클로즈드 서비스
- 기술 스택: React/Next.js + Vercel

## Offline Engine

- 정부 폐쇄망 환경을 위한 오프라인 엔진
- [[Thales-Envelope]]를 사용하여 .pyc 파일 난독화
- GS 인증이 정부 마켓플레이스 진입에 필요

## Related

- [[Thales-Envelope]]
- [[Photogrammetry]]
