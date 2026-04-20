---
title: Oracle Cloud SaaS 검토 요약
type: summary
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-ARCHIVE-E8.md
tags:
  - 클라우드
  - Oracle
  - 인프라
---

# Oracle Cloud SaaS 검토

## AWS 대비 특장점

- GPU 가격 최적화
- 견적 오차 ±10%로 적은 편차
- 10TB 트래픽 무료
- Storage 비용 AWS의 1/4
- CPU/GPU/메모리 자유 조정 (CPU만 100코어 가능)
- 비즈니스 규모 증가에 따른 비용이 선형적
- ISMS 별도 취득 불필요 (ISMS-P는 별도)
- KINS 망 인프라 사용 가능

## AI 서비스

- Baremetal: A100/H100 (직접 학습용), 1~3년 게런티시 할인
- VM형태: On/Off 가능, Compact
- 학습 없이 추론 전용 장비 별도 제공
- 슈퍼클러스터링 제공

## 약점

- AI/ML 기능은 AWS 대비 약함
- 마켓플레이스 부족

## NFLOW 적용 검토

- Gen APP 형태로 NFLOW 제공 가능
- 쿠버네티스 서비스 제공 가능
- RDMA 100GB 송수신 가능
- VCN 기반 논리적 망분리 가능

## 관련

- [[NFLOW]]
