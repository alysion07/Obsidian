---
title: Thales Envelope
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-PIX4D-AREA.md
tags:
  - 라이선싱
  - 보안
  - 난독화
---

# Thales Envelope

Thales LMS(License Management System)의 Envelope 도구로, Python .pyc 파일을 난독화하는 데 사용된다.

## 동작 프로세스

1. H/W key 생성
2. .pyc 파일과 python310.dll을 페어링하여 암호화
3. 암호화된 interpreter에서만 난독화된 .pyc 구동 가능

## 주요 특성

- 암호화된 interpreter에서 일반 .pyc 파일도 정상 사용 가능
- 난독화된 .pyc는 decompile 불가능
- Port 1947 방화벽 예외 설정 필요

## 용도

PIX4D Engine SDK의 정부 폐쇄망 오프라인 배포에 사용된다. 소스 코드 보호와 라이선스 관리를 동시에 달성할 수 있다.

## Related

- [[PIX4D]]
