---
title: NFLOW
type: entity
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-ARCHIVE-E8.md
tags:
  - CFD
  - 시뮬레이션
  - 제품
  - E8
---

# NFLOW

## 개요

E8ight에서 개발하는 CFD(전산유체역학) 시뮬레이션 소프트웨어.

## 프로모션 (2025.07.07~)

- 60일 무료 사용 프로모션 진행
- 모든 솔버 사용 가능, 기능 제약 없음
- 클라우드 라이선스 방식
- 프로모션 신청 현황: 학교 7건, 기업 3건 (총 10건)

## 납품 이력

- 경북대학교
- 현대차 (8월 말 예정)

## 라이선스

- Feature ID 기준으로 라이선스 수집 여부 결정
- 등급별 기능 정리 문서 존재

## 로깅 / 모니터링

- [[Sentry]] 연동: Crashpad 기반 크래시 리포팅
- Breadcrumb: 다이얼로그 Open, 에디터 조작 등
- captureMessage: 다이얼로그 Close, 리본 메뉴 클릭
- captureError/Exception: Try/Catch, StackTrace 활용
- Attachment 기능으로 미니 덤프 파일 전송 가능

## JSON 검증기

- 시뮬레이션 설정 파일의 JSON 구조를 검증
- C++ 기반, nlohmann/json + valijson 라이브러리
- JSON Schema Draft 7 사용

## NFLOW AI-TFT

- GS 인증 대상
- Workflow: 해석 → Sampling 설정 → Target 설정 → 2D VTK 추출
- AI 커널 입력을 최소화하여 일반인도 조작 가능하도록 기획

## 상용 CFD 대비 기능 비교

- ANSYS Fluent: Wall Shear Stress 기반 분리 감지, 자동 보고서
- Siemens STAR-CCM+: Iso-Surface 자동 생성, 경계층 분석
- OpenFOAM + ParaView: Python 스크립트 기반 후처리
- Autodesk CFD: 압력 구배 기반 분리 표시

## 관련

- [[RS-Engine]]
- [[Sentry]]
- [[연합트윈]]
