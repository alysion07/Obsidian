# Activity Log

위키 활동을 시간순으로 기록합니다.

---

## 2026-04-14

- `14:30` **INIT** 위키 초기 구조 생성 - CLAUDE.md, index.md, log.md, 디렉토리 구조 셋업 완료
- `15:10` ~~**INGEST** `업무 스케쥴 초안.md`~~ → 삭제됨 (소스 및 파생 페이지 모두 제거)
- `15:30` **INGEST** `AREA/PIX4D/` → 엔티티 1개(PIX4D), 개념 4개(Photogrammetry, Thales-Envelope, PIX4D-Config-Guide, BIM) 생성
- `15:30` **INGEST** `AREA/V-SMR/` → 요약 1개(VSMR 플랫폼), 엔티티 3개(MARS-GUI, iPWR, 연합트윈), 개념 1개(SMR) 생성
- `15:30` **SKIP** `AREA/Gitlab 정보.md` - 비밀번호/계정 포함, `AREA/면담 자료/` - 개인정보
- `15:50` **INGEST** `ARCHIVE/E8/` → 엔티티 3개(NFLOW, RS-Engine, Sentry), 요약 2개(POSCO 철근배근, Oracle SaaS) 생성. 주간회의/교류회(시간성), THALES Feature(비밀번호) 제외
- `16:00` **INGEST** `RESOURCE/GRAPHICS/` → 개념 3개(Computer-Graphics, Deferred-Rendering, WebGL2) 생성
- `16:00` **INGEST** `RESOURCE/LANGUAGE+NETWORKS/` → 개념 6개(JavaScript, TypeScript, React, Three-js, Networks, WebAssembly) 생성
- `16:00` **INGEST** `RESOURCE/기타/` → 개념 7개(Gaussian-Splatting, Computer-Engineering, Algorithm, Digital-Twin, NURBS, Event-Sourcing, NeRF) 생성
- `16:00` **SKIP** `RESOURCE/CARS, COFFEE, STUDY, Q.A., 주식` - 비기술 자료 제외
- `16:20` **BACKUP** 원본 vault 전체를 `sources/raw/`에 복사 (512파일, 48MB, 이미지/첨부 포함)
- `17:10` **INGEST** `VSMR 변경이력 (1b34bff→bf71755)` → 요약 1개(VSMR-변경이력-1b34bff-bf71755) 생성, MARS-GUI 엔티티 업데이트

## 2026-04-15

- `09:00` **INGEST** `VSMR 시뮬레이션 결과 저장경로 (BFF 전달)` → 요약 1개(VSMR-시뮬레이션결과-저장경로) 생성, MARS-GUI 엔티티 업데이트

## 2026-04-20

- `09:30` **INGEST** `level-set-topology-optimization.pdf/.docx` (Level Set 기반 위상 최적화 Theory Note) → 요약 1개(Level-Set-Topology-Optimization), 개념 5개(Level-Set-Method, Topology-Optimization, Hamilton-Jacobi-Equation, Signed-Distance-Function, Eikonal-Equation) 생성
- `10:00` **REBUILD** `level-set-topology-optimization.pdf` 요약 페이지 재생성 (삭제되었던 `wiki/summaries/Level-Set-Topology-Optimization.md` 복원)
