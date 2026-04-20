---
title: MARS GUI 전처리기
type: entity
created: 2026-04-14
updated: 2026-04-15
sources:
  - sources/2026-04-14-VSMR-AREA.md
  - sources/2026-04-14-VSMR-변경이력-1b34bff-bf71755.md
  - sources/2026-04-15-VSMR-시뮬레이션결과-저장경로-BFF.md
tags:
  - MARS
  - GUI
  - ReactFlow
  - 시뮬레이션
  - 전처리기
---

# MARS GUI 전처리기

- React Flow 기반 MARS 전처리 GUI 시스템
- 목표: SMART.i 기준 MARS 입력 덱 생성을 위한 웹 기반 전처리기
- 11개 MARS 컴포넌트: snglvol, pipe, sngljun, mtpljun, branch, pump, valve, prizer, sg, accumulator, eccmixer
- 자동 CCC 번호 관리 (시스템별 범위: 100-199 Primary, 200-299 Secondary 등)
- Tech stack: React 18+, TypeScript 5+, React Flow 11+, Zustand 4+, Vite, Tailwind CSS
- Architecture: Presentation Layer (React+Flow) -> Business Logic (Validation/Export/Numbering) -> Data Layer (Zustand) -> Type Layer (TypeScript)
- 7 Phase plan (10주): Phase 1 완료 (기초 인프라), Phase 2~7 진행 중
- 106 tests passing (MarsValidator 14, CCCManager 37, EccmixerForm 17, SgForm 18, ValveForm 20)
- Related: [[V-SMR]], [[SMR]]

## 변경이력

- [[VSMR-변경이력-1b34bff-bf71755]]: QuickRun 검증 플로우 도입, precice_mars.nml URL 지원, 히스토리 로드 리팩터링, TaskListPanel 캐싱
