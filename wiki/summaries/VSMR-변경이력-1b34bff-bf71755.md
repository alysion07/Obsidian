---
title: VSMR 변경이력 (1b34bff → bf71755)
type: summary
created: 2026-04-14
updated: 2026-04-14
sources: [sources/2026-04-14-VSMR-변경이력-1b34bff-bf71755.md]
tags: [VSMR, changelog, QuickRun, validation, precice]
---

# VSMR 변경이력: 1b34bff → bf71755

커밋 2건, 파일 10개 변경 (+586 / -161). QuickRun 검증 시스템 도입과 precice 연동 개선이 핵심.

## 핵심 변경 요약

### 입력 파일 검증 시스템 (신규)

Proto 스키마에서 `ValidationError` 메시지 타입을 제거하고 `repeated string messages`로 단순화. `inputdService.ts`에 `validateInputContent()` RPC 클라이언트를 새로 구현하고, `InputValidationResultDialog.tsx`로 PASS/FAIL 결과를 모노스페이스 로그 형태로 표시한다.

### QuickRun 2단계 실행 구조

`useCoSimQuickRun.ts`를 대폭 개선. 검증(validateMutation) → 실행(startMutation)으로 분리하고, 모델별 검증 상태(idle/validating/valid/invalid)를 관리한다. FNV-1a 해시로 입력 파일 변경을 감지하여 미변경 시 재검증을 스킵하며, `canExecute` 플래그로 실행 가능 여부를 제어한다.

### precice_mars.nml URL 지원

업로드 시 `precice_mars.nml` 파일 URL을 별도 추출하여 Build 요청의 3번째 인자로 전달. 단일/멀티 모델 모두 `[taskMode, inputFileUrl, preciceMarsNmlUrl?]` 형태로 통일했다.

### 시뮬레이션 히스토리 로드 리팩터링

`ProjectHomePage.tsx`에서 plotfl 다운로드/파싱 로직을 전부 제거하고 `SimulationPage.tsx`(AnalysisView)로 이동. `requestedSimulationId` query param 기반으로 자동 로드하며, 멀티 모델은 taskIndex 순회, 단일/quickrun은 taskIndex 0 fallback.

### TaskListPanel 캐싱

5초 TTL 메모리 캐시와 in-flight 중복 요청 병합을 추가. 새로고침은 `force=true`로 캐시를 무시한다.

### 기타

- 에러 toast에서 상세 내용 노출 제거 (보안/UX)
- mutation `retry: false`로 자동 재시도 방지
- 검증 실패 시 `InputValidationResultDialog` 자동 표시

## 관련 페이지

- [[MARS-GUI]]
- [[VSMR-플랫폼-개발-사업]]
