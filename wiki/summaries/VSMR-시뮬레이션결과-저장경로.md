---
title: VSMR 시뮬레이션 결과 저장 경로
type: summary
created: 2026-04-15
updated: 2026-04-15
sources: [sources/2026-04-15-VSMR-시뮬레이션결과-저장경로-BFF.md]
tags: [VSMR, simulation, storage, BFF, task_index]
---

# VSMR 시뮬레이션 결과 저장 경로

BFF 담당자 전달 내용. 시뮬레이션 결과의 저장 경로 구조와 task_index 사용 이유를 정리.

## 저장 경로

```
v-smr/[사용자 uuid]/[project-id]/simulation/history/[simulation-id]/{task_index}/{출력파일}
```

## task_index와 model-id의 관계

- **task_index를 쓰는 이유**: mars-adaptor에서는 model-id를 알 수 없기 때문
- **task_index = model 순서**: 0이면 첫번째 모델, 1이면 두번째 모델
- **restart 시 주의**: 설정에는 project_id, simulation_id, model_id를 입력받지만, 실제 파일 접근 시에는 task_index가 필요
- BFF 담당자가 task_index를 model 순서로 매핑하도록 수정 예정

## 최근 결과 저장 폐지

기존에 최근 결과를 별도 경로에 저장하던 동작은 폐지됨. 이제 history 경로만 사용.

## 관련 페이지

- [[MARS-GUI]]
- [[VSMR-변경이력-1b34bff-bf71755]] — 히스토리 로드 리팩터링에서 이 경로 구조를 사용
