# 목적

브라우저에서 CAD → 전처리(BC) → 해석 실행 → 후처리(가시화)까지 제공하는 **MVP**. 팀 공유용 큰 그림.

  

---

  

# 1) MVP 범위(사용자 스토리)

- **모델 업로드**: STEP/IGES/STL(서버 변환) 또는 glTF/GLB 직접 업로드.

- **전처리**: 대형 모델 로딩, LOD 렌더, 피킹, 경계조건(BC) 태깅, 프로젝트 저장/불러오기.

- **해석 실행**: 클라우드/온프리미스 K8s에서 컨테이너 솔버 1종 실행, 진행률/로그 확인.

- **후처리**: VTK 계열 결과 부분 로딩, 슬라이스/등가면 가시화, 스크린샷 저장.

- **운영**: 계정/RBAC, 워크스페이스, 사용량 기본 대시보드.

  

---

  

# 2) 서비스 워크플로우(상위 시퀀스)

1. 사용자가 프로젝트 생성 → CAD 업로드.

2. 서버가 CAD→메시/중간포맷 변환(필요 시) → S3(MinIO)에 저장.

3. 프런트가 glTF(+KTX2/meshopt) 로드 → LOD 렌더 → 피킹/BC 편집.

4. 사용자가 **Job 제출**: 입력 세트(ProjectFile+Case) 고정 → JobSpec 생성.

5. **Job 서비스**가 K8s Runner에 제출. 라이선스 사이드카가 Sentinel 확인.

6. 솔버가 S3에서 입력 pull → 계산 → 결과를 S3에 push.

7. **Result 서비스**가 결과 인덱스(Zarr/브릭 메타) 생성.

8. 프런트가 워커/IndexedDB 캐시로 부분 로딩 → 후처리 렌더.

  

---

  

# 3) 시스템 아키텍처(논리)

- **프런트엔드**: React + Vite + TypeScript + three.js. Zustand(상태), React Query(I/O), Dexie(IndexedDB 캐시), Web Worker + WASM.

- **API 게이트웨이**: REST/GraphQL. 인증/권한, 속도제한, 프리사인 URL 발급.

- **서비스들**

  - **Auth**: Keycloak 기반 사용자/토큰/RBAC.

  - **Project**: 프로젝트/버전/권한. ProjectFile 스키마 마이그레이션.

  - **Asset**: 업로드, gltfpack/meshopt/KTX2 전처리 파이프라인.

  - **Job**: 큐/스케줄/상태 머신. 재시도/타임아웃. 사용량 기록.

  - **Compute-Agent**: K8s Job/Pod 관리, Sentinel Mixed 체크(license-sidecar).

  - **Result**: VTK→Zarr 브리킹, 인덱스/통계 메타 생성.

- **데이터**: MinIO(S3 객체), Postgres(메타), Redis(큐/세션), Loki/Prom/Grafana(관측성).

- **배포**: K8s. 프런트는 CDN(또는 Nginx) 정적 호스팅.

  

```

[Browser] ───HTTP(S)──▶ [API GW] ──▶ [Auth/Project/Asset/Job/Result]

     ▲                                 │             │

     │                                 │             ├──▶ [MinIO S3]

     │                                 │             └──▶ [Postgres/Redis]

     │                                 └──▶ [Compute-Agent] ─▶ [K8s Runner + License-Sidecar]

     │                                                               │

     └────────── 워커/IndexedDB ◀── 결과(Zarr/VTK) ◀── S3 ────────────┘

```

  

---

  

# 4) 코어 "계약"(Contracts)

## DrawList/SceneIndex(렌더 공통)

- `SceneIndex`: 타일/노드/LOD/MaterialKey 카탈로그.

- `DrawList`: 프레임마다 컬링→LOD→배칭 결과.

  

## JobSpec v1(해석 실행)

```json

{

  "projectId":"p123","caseId":"c001","solver":"nflow-lbm:3.2",

  "inputs":[{"path":"s3://bucket/p123/cases/c001/case.json"}],

  "resources":{"cpu":8,"mem":"16Gi","gpu":0,"walltime":"02:00:00"},

  "env":{"FEATURE_ID":"4000"},

  "artifacts":{"stdout":true,"resultsPrefix":"s3://bucket/p123/runs/2025-08-27/"}

}

```

  

## ProjectFile v3(전처리 산출)

```json

{

  "version":3,

  "meta":{"id":"p123","user":"u1","created":"...","updated":"..."},

  "model":{"gltf":"s3://.../asset.glb","lod":[{"sseIn":20,"sseOut":10}]},

  "bc":[{"id":"in1","target":{"type":"face","ids":[1,2,3]},"tag":"inlet","params":{"U":1.2}}],

  "post":{"datasets":[{"id":"r1","uri":"s3://.../vtk.zarr","kind":"scalar3d"}]}

}

```

  

---

  

# 5) 프런트엔드 뼈대(전처리/후처리 공통)

- **Viewport3D(three.js)**: 프레임워크와 독립. 입력은 DrawList만.

- **Visibility→LODSelector→Batcher→DrawList** 파이프라인.

- **피킹**: three-mesh-bvh + 오프스크린 ID Buffer.

- **상태**: Zustand(선택/카메라/플래그), React Query(I/O), Command 패턴(Undo/Redo).

- **파일 I/O**: MinIO 프리사인 URL, Dexie 캐시.

  

---

  

# 6) 후처리 데이터 경로

- Worker+WASM 파서 → Dexie 캐시 → 3D 브릭 업로드.

- 슬라이스/등가면(타일 단위 marching cubes), 스크린샷 저장.

- 메모리 예산: VRAM/RAM 한도 내 동적 해상도/LOD 조정.

  

---

  

# 7) 제공 방식(서비스 모델)

- 1단계: **프라이빗 온프리미스** 또는 VPC 내부 SaaS. 단일 테넌트 워크스페이스.

- 2단계: 다중 테넌트 SaaS(코어시간 기반 과금) 옵션.

- 인증/권한: Keycloak. 프로젝트 단위 RBAC.

  

---

  

# 8) 업무 분장(워크스트림)

## FE(전처리/후처리)

- Viewport3D, DrawList 파이프라인, 피킹/오버레이, Post 브릭 뷰어, UI 폼/검증.

## BE(서비스)

- Auth/Project/Asset/Job/Result API, 프리사인 URL, Zarr 인덱서.

## Infra/DevOps

- K8s/MinIO/Postgres/Redis/Keycloak 배포. CI/CD. 관측성. 스토리지 정책.

## Solver 통합

- 컨테이너 이미지, 엔트리포인트, 입력/출력 스키마 정합, Sentinel Mixed 연동.

## UX/PM

- 화면 플로우, 폼 밸리데이션, 튜토리얼, 문서/릴리즈 노트.

  

---

  

# 9) 마일스톤(8개월 로드맵)

- **M1(0–4주)**: 계약 고정(SceneIndex/DrawList/JobSpec), 템플릿, Auth/RBAC.

- **M2(5–12주)**: 전처리 MVP(LOD/피킹/BC), Asset 파이프라인, 저장/불러오기.

- **M3(13–20주)**: Job 서비스+K8s Runner+Sentinel, 단일 솔버 실행/모니터링.

- **M4(21–28주)**: 후처리 스트리밍(Zarr/브릭), 슬라이스/등가면, 캐시/예산기.

- **M5(29–32주)**: 통합 안정화, 대시보드, SLO 달성(전처리 55–60fps, 10GB 결과 부분가시화).

  

---

  

# 10) 수용기준(KPI)

- **전처리**: 5M+ 트라이 모델에서 60fps 유지, 클릭→하이라이트 < 16ms.

- **해석**: 단일 케이스 제출→완료까지 자동화. 실패 시 재시도/로그 수집.

- **후처리**: 10GB 결과를 부분 로딩으로 시각화, 브라우저 OOM 없음.

  

---

  

# 11) 위험과 대응

- 브라우저 메모리 한계 → Out-of-core(브리킹/스트리밍) 기본.

- 대형 모델 프레임 저하 → HLOD+동적 해상도, 배칭 규약.

- 라이선스 의존 → license-sidecar 표준화, 호스트 매핑 체크리스트.

- 다테넌시 보안 → S3 Prefix 정책, 네임스페이스 격리, 자동화 테스트.

  

---

  

# 12) 의사결정 규칙(초기 고정)

- **DrawList/SceneIndex**는 프레임워크와 분리. 테스트 가능 모듈.

- **ProjectFile/JobSpec** 스키마는 버저닝과 마이그레이션 제공.

- 프런트는 **렌더러 교체(WebGPU)** 에 대비해 DrawList 포맷 유지.

- 서버는 **S3 프리사인 URL**을 기본. 마운트 방식은 옵션.

  

---

  

# 13) 참고 구현 스택

- FE: React, three.js, Zustand, React Query, Dexie, shadcn/ui, lucide-react.

- BE: Node/Express 또는 FastAPI, Postgres, Redis, MinIO SDK, Keycloak, OpenAPI.

- DevOps: K8s, ArgoCD/GitLab CI, Prometheus+Loki+Grafana, Sentry.