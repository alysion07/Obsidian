---
title: Sentry
type: entity
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-ARCHIVE-E8.md
tags:
  - 모니터링
  - 에러추적
  - Crashpad
---

# Sentry

## 개요

애플리케이션 에러 추적 및 모니터링 플랫폼. [[NFLOW]]에서 크래시 리포팅과 사용자 행동 추적에 활용.

## NFLOW 연동

### Crashpad
- C/C++ 네이티브 크래시 핸들러
- `sentry_options_set_handler_path`로 crashpad 경로 설정
- `sentry_options_set_database_path`로 DB 경로 설정

### Attachment 활용
- 글로벌 범위: `sentry_options_add_attachment` (모든 이벤트에 첨부)
- 로컬 범위: `sentry_scope_attach_file` (특정 이벤트에 첨부)
- 바이트 변수: `sentry_attach_bytes`
- 미니 덤프 파일 전송에 활용

## 관련

- [[NFLOW]]
