
> React Flow 기반 MARS 전처리 GUI 시스템 구현 계획 (Phase 재구성 버전)

## 🎯 프로젝트 개요

### 목표

- SMART.i 기준 MARS 입력 덱 생성을 위한 웹 기반 전처리기 개발
- 노드/엣지 그래픽 UI를 통한 직관적인 계통 모델링
- MARS CCC 카드 구조 자동 생성 및 검증
- MARS  Input Manuel 기반 노드간 연결 규칙 검증 
- 실시간 유효성 검사 및 오류 방지
- tailwind를 통한 디자인 적용

### 핵심 기능

- **11개 MARS 컴포넌트 타입 지원**: snglvol, pipe, sngljun, mtpljun, branch, pump, valve, prizer, sg, accumulator, eccmixer 등
- **자동 CCC 번호 관리**: 시스템별 번호 범위 (100-199: Primary, 200-299: Secondary 등)
- **실시간 연결 검증**: from/to 규칙, phase 매칭, 방향성 제약 검사
- **MARS 포맷 자동 생성**: 슬롯 매핑을 통한 CCC 카드 구조 출력

### Phase 재구성 전략

**기존 계획**: Phase 1 → 2 → 3 → 4 → 5 → 6 (12주) **재구성 계획**: Phase 1(완료) → 2(최소화) → 3(우선) → 4(신규) → 5 → 6 → 7 (10주)

- Phase 3 (고급 연결 시스템)을 우선 구현
- Phase 2를 최소화하여 Phase 3 지원에 필요한 기능만 구현
- 복잡한 컴포넌트 구현을 새로운 Phase 4로 이동

---

## 🏗️ 시스템 아키텍처

### 계층 구조

![](http://localhost:63342/markdownPreview/314627813/)

`┌─────────────────────────────────────┐ │        Presentation Layer           │ │     (React + React Flow UI)         │ ├─────────────────────────────────────┤ │        Business Logic Layer         │ │  (Validation, Export, Numbering)    │ ├─────────────────────────────────────┤ │          Data Layer                 │ │      (Zustand State Stores)         │ ├─────────────────────────────────────┤ │          Type Layer                 │ │    (TypeScript Definitions)         │ └─────────────────────────────────────┘`

### 핵심 설계 결정

- **React Flow**: 검증된 그래프 시각화 라이브러리
- **Zustand**: 가벼운 상태 관리 (Redux 대비 React Flow 호환성 우수)
- **TypeScript**: 타입 안전성으로 복잡한 MARS 규칙 관리
- **커스텀 검증 엔진**: MARS 고유 규칙 처리
- **실시간 검증**: 사용자 경험 최적화

---

## 📂 파일 구조 및 모듈 조직

### 전체 디렉토리 구조

![](http://localhost:63342/markdownPreview/314627813/)

`mars-gui/ ├── public/ ├── src/ │   ├── components/              # React 컴포넌트 │   │   ├── nodes/              # MARS 노드 컴포넌트 │   │   ├── edges/              # 연결선 컴포넌트 │   │   ├── ui/                 # 공통 UI 컴포넌트 │   │   └── canvas/             # 캔버스 관련 컴포넌트 │   ├── engine/                 # 비즈니스 로직 │   │   ├── validation/         # 검증 엔진 │   │   ├── export/             # MARS 출력 엔진 │   │   ├── numbering/          # CCC 번호 관리 │   │   └── graph/              # 그래프 연산 │   ├── store/                  # 상태 관리 │   ├── types/                  # 타입 정의 │   ├── utils/                  # 유틸리티 함수 │   ├── hooks/                  # 커스텀 훅 │   └── App.tsx                 # 메인 애플리케이션 ├── tests/                      # 테스트 파일 ├── docs/                       # 문서 └── package.json`

### 🎨 주요 컴포넌트 파일

#### Node 컴포넌트 (src/components/nodes/)

![](http://localhost:63342/markdownPreview/314627813/)

`SnglvolNode.tsx          # 단일 체적 노드 PipeNode.tsx             # 파이프 노드 (다중 셀 지원) PumpNode.tsx             # 펌프 노드 (inlet/outlet + 곡선) ValveNode.tsx            # 밸브 노드 (트립 로직 지원) BranchNode.tsx           # 분기 노드 (다중 junction) MtpljunNode.tsx          # 다중 접합 노드 EccMixerNode.tsx         # ECC 혼합기 (3-port 필수) PrizerNode.tsx           # 가압기 노드 SgNode.tsx               # 증기발생기 노드 AccumulatorNode.tsx      # 축압기 노드 TmdpvolNode.tsx          # 시간의존 체적 TmdpjunNode.tsx          # 시간의존 접합 CircltrNode.tsx          # 순환기 노드 BaseNode.tsx             # 공통 노드 기능 NodeFactory.tsx          # 노드 생성 팩토리`

#### Edge 컴포넌트 (src/components/edges/)

![](http://localhost:63342/markdownPreview/314627813/)

`MarsEdge.tsx             # MARS 연결선 (from/to 표시) ConnectionValidator.tsx   # 연결 유효성 실시간 검사 EdgeFactory.tsx          # 엣지 타입 결정 로직 JunctionHandler.tsx      # junction 매개변수 처리`

#### UI 컴포넌트 (src/components/ui/)

![](http://localhost:63342/markdownPreview/314627813/)

`ComponentPalette.tsx     # 컴포넌트 팔레트 (드래그 앤 드롭) PropertiesPanel.tsx      # 노드/엣지 속성 패널 ValidationPanel.tsx      # 검증 결과 표시 패널 ExportPreview.tsx        # MARS 출력 미리보기 ParameterForm.tsx        # 동적 매개변수 입력 폼 ErrorTooltip.tsx         # 오류 툴팁 컴포넌트 ProgressIndicator.tsx    # 진행 상황 표시`

#### Canvas 컴포넌트 (src/components/canvas/)

![](http://localhost:63342/markdownPreview/314627813/)

`FlowCanvas.tsx           # 메인 React Flow 캔버스 Toolbar.tsx              # 도구 모음 (저장/불러오기 등) GridBackground.tsx       # 격자 배경 MiniMap.tsx              # 미니맵 컴포넌트`

### ⚙️ 비즈니스 로직 파일 (src/engine/)

#### 검증 엔진 (src/engine/validation/)

![](http://localhost:63342/markdownPreview/314627813/)

`ValidationEngine.ts      # 메인 검증 엔진 ConnectionRules.ts       # 연결 규칙 (from/to, phase, 방향) ComponentRules.ts        # 컴포넌트별 규칙 MarsRules.ts             # MARS 고유 규칙 PortConflictChecker.ts   # 포트 충돌 검사 PhaseValidator.ts        # phase 매칭 검증 GeometryValidator.ts     # 기하 매개변수 검증`

#### 출력 엔진 (src/engine/export/)

![](http://localhost:63342/markdownPreview/314627813/)

`MarsExporter.ts          # 메인 출력 엔진 CardGenerator.ts         # CCC 카드 생성기 SlotMapper.ts            # 슬롯 매핑 (PLAN.md 기준) ComponentExporter.ts     # 컴포넌트별 출력 로직 JunctionExporter.ts      # 접합 출력 로직 FormatValidator.ts       # 출력 포맷 검증`

#### CCC 번호 관리 (src/engine/numbering/)

![](http://localhost:63342/markdownPreview/314627813/)

`CCCNumbering.ts          # CCC 번호 할당/관리 SystemRanges.ts          # 시스템별 번호 범위 ConflictResolver.ts      # 번호 충돌 해결 NumberGenerator.ts       # 자동 번호 생성`

#### 그래프 연산 (src/engine/graph/)

![](http://localhost:63342/markdownPreview/314627813/)

`GraphOperations.ts       # 그래프 기본 연산 PortManager.ts           # 포트 관리 (생성/삭제/연결) ConnectionManager.ts     # 연결 관리 NodeLifecycle.ts         # 노드 생명주기 관리`

### 💾 상태 관리 파일 (src/store/)

![](http://localhost:63342/markdownPreview/314627813/)

`graphStore.ts            # 노드/엣지 상태 validationStore.ts       # 검증 결과 상태   exportStore.ts           # 출력 설정 상태 uiStore.ts               # UI 상태 (선택, 패널 등) numberingStore.ts        # CCC 번호 할당 상태 historyStore.ts          # Undo/Redo 상태`

### 📋 타입 정의 파일 (src/types/)

![](http://localhost:63342/markdownPreview/314627813/)

`mars-types.ts            # MARS 노드/엣지 타입 component-schemas.ts     # 컴포넌트별 스키마 validation-types.ts      # 검증 관련 타입 export-types.ts          # 출력 관련 타입 ui-types.ts              # UI 상태 타입 port-types.ts            # 포트 시스템 타입`

### 🛠️ 유틸리티 파일 (src/utils/)

![](http://localhost:63342/markdownPreview/314627813/)

`mars-utils.ts            # MARS 전용 유틸리티 graph-utils.ts           # 그래프 조작 유틸리티 validation-utils.ts      # 검증 헬퍼 함수 format-utils.ts          # 포맷 변환 유틸리티 port-utils.ts            # 포트 관리 유틸리티`

### 🎣 커스텀 훅 (src/hooks/)

![](http://localhost:63342/markdownPreview/314627813/)

`useValidation.ts         # 검증 상태 관리 useExport.ts             # 출력 기능 관리 useGraphOperations.ts    # 그래프 연산 훅 useNodeConnection.ts     # 노드 연결 관리 useParameterForm.ts      # 매개변수 폼 관리 useCCCNumbering.ts       # CCC 번호 관리`

---

## 🔧 기술 스택

### 핵심 기술

|기술|버전|목적|
|---|---|---|
|React|18+|UI 프레임워크|
|TypeScript|5+|타입 안전성|
|React Flow|11+|그래프 시각화|
|Vite|4+|빌드 도구|
|Zustand|4+|상태 관리|

### UI 및 스타일링

|기술|목적|
|---|---|
|Tailwind CSS|유틸리티 스타일링|
|Headless UI|접근성 컴포넌트|
|React Hook Form|복잡한 폼 관리|
|Radix UI|고급 UI 컴포넌트|

### 개발 도구

|기술|목적|
|---|---|
|ESLint + Prettier|코드 품질|
|Jest + RTL|테스트|
|Storybook|컴포넌트 개발|
|Zod|런타임 검증|

---

## 📋 단계별 구현 계획

### Phase 1: 기초 인프라 ✅ **완료** (2025-09-17)

**목표**: 프로젝트 기반 구조 및 기본 React Flow 통합

#### 주요 작업

- [x] 프로젝트 초기화 (Vite + React + TypeScript)
- [x] React Flow 11.11.4 기본 설정 및 캔버스 구현
- [x] Zustand 5.0.8 상태 관리 구조
- [x] 핵심 타입 정의 (`mars-types.ts`, `component-schemas.ts`)
- [x] 11개 MARS 컴포넌트 기본 구현
- [x] CCC 자동 번호 매기기 시스템 구현

#### 완료 상태

- ✅ React Flow 캔버스가 정상 작동
- ✅ 11개 모든 MARS 노드 생성/삭제 가능
- ✅ TypeScript 타입 시스템 완전 구축
- ✅ CCC 번호 자동 할당 작동
- ✅ 기본 테스트 환경 구성 완료 (Vitest + RTL)

---

### Phase 2: 최소 노드 시스템 준비 (주 1) 🎯 **다음 목표**

**목표**: Phase 3 연결 시스템을 위한 최소한의 노드 기능 구현

#### 주요 작업

- [ ] 포트 시스템 기초 구현
    - [ ] inlet/outlet 포트 정의
    - [ ] 포트별 handle 위치 설정
    - [ ] 다중 포트 지원 구조
- [ ] React Flow handle 구현
    - [ ] 연결점 시각화
    - [ ] handle ID 체계 구축
    - [ ] 포트별 스타일 구분
- [ ] 노드별 포트 구성 정의
    - [ ] 단일 inlet/outlet 노드 (snglvol, pipe)
    - [ ] 다중 포트 노드 (branch, mtpljun)
    - [ ] 특수 포트 노드 (eccmixer 3-port)

#### 완료 기준

- ⏳ 모든 노드에 포트 시스템 적용
- ⏳ 연결 가능한 handle 표시
- ⏳ 포트 타입별 시각적 구분
- ⏳ 기본 연결 테스트 통과

---

### Phase 3: 고급 연결 시스템 (주 2-3) 🔄 **우선 구현**

**목표**: MARS 규칙 기반 고급 연결 검증 및 매개변수 관리

#### 주요 작업

**Week 2**: 연결 검증 시스템

- [ ] Phase 매칭 검증
    - [ ] liquid↔liquid 연결만 허용
    - [ ] gas↔gas 연결만 허용
    - [ ] mix phase 특수 처리
- [ ] 포트 용량 제한
    - [ ] inlet: 단일 연결만 허용
    - [ ] outlet: 다중 연결 허용
    - [ ] 포트별 최대 연결 수 검사
- [ ] 방향성 검증
    - [ ] out→in 방향만 허용
    - [ ] 역방향 연결 방지

**Week 3**: 연결 매개변수 관리

- [ ] 연결 매개변수 UI
    - [ ] area (유동 면적)
    - [ ] kfor/krev (압력 손실 계수)
    - [ ] flags (특수 옵션)
- [ ] 실시간 연결 피드백
    - [ ] 유효 연결 녹색 표시
    - [ ] 무효 연결 빨간색 표시
    - [ ] 오류 메시지 툴팁
- [ ] Junction 매개변수 처리
    - [ ] junction 번호 할당

#### 완료 기준

- ⏳ MARS 규칙에 따른 연결만 가능
- ⏳ 실시간 검증 피드백 제공
- ⏳ 연결 매개변수 편집 가능
- ⏳ 명확한 오류 메시지 표시

---

### Phase 4: 컴포넌트별 상세 구현 (주 4-5) 📦 **새로 추가**

**목표**: 복잡한 MARS 컴포넌트 기능 완성

#### 주요 작업

**Week 4**: 복잡한 컴포넌트 구현

- [ ] PumpNode 고급 기능
    - [ ] H-Q 곡선 입력 UI
    - [ ] inlet/outlet 포트 곡선
    - [ ] 펌프 특성 매개변수
- [ ] ValveNode 트립 로직
    - [ ] 트립 조건 설정
    - [ ] 밸브 타입별 동작
    - [ ] 제어 로직 구현

**Week 5**: 특수 컴포넌트 완성

- [ ] BranchNode 다중 접합
    - [ ] 가변 junction 수
    - [ ] 동적 포트 생성
- [ ] EccMixerNode 3-port
    - [ ] 필수 3개 포트 검증
    - [ ] 혼합 비율 설정
- [ ] 테이블 데이터 처리
    - [ ] 시간 의존 데이터
    - [ ] 곡선 데이터 입력

#### 완료 기준

- ⏳ 모든 컴포넌트 고급 기능 구현
- ⏳ 복잡한 매개변수 폼 완성
- ⏳ 테이블 데이터 입력 가능
- ⏳ 컴포넌트별 검증 규칙 적용

---

### Phase 5: MARS 검증 엔진 (주 6-7)

**목표**: 종합적인 MARS 규칙 검증 시스템

**현재 테스트 현황 (106 tests passing)**:

- MarsValidator: 14 tests ✅
- CCCManager: 37 tests ✅
- EccmixerForm: 17 tests ✅
- SgForm: 18 tests ✅
- ValveForm: 20 tests ✅

#### 주요 작업

**Week 6**: 기본 검증 완성

- [x] ValidationEngine 메인 클래스 ✅
- [x] CCC 번호 중복 검사 ✅
- [x] 포트 충돌 검사 ✅
- [ ] 컴포넌트별 필수 매개변수 검사
- [ ] 그래프 연결성 검사 확장

**Week 7**: MARS 고유 규칙

- [ ] 금지 조합 검사 (매뉴얼 기준)
- [ ] 물리적 제약 검사
    - [ ] 기하 매개변수 범위
    - [ ] 압력/온도 한계
    - [ ] 유량 제약
- [ ] 실시간 검증 결과 UI
    - [ ] ValidationPanel 구현
    - [ ] 오류/경고 분류
    - [ ] 수정 제안 표시

#### 완료 기준

- ⏳ 모든 MARS 규칙 자동 검증
- ⏳ 실시간 검증 피드백
- ⏳ 검증 실패 시 출력 방지
- ⏳ 명확한 오류 가이드 제공

---

### Phase 6: MARS 출력 시스템 (주 8-9)

**목표**: MARS 포맷 출력 및 파일 생성

#### 주요 작업

**Week 8**: 기본 출력 기능

- [ ] MarsExporter 메인 클래스
- [ ] CCC 카드 생성기
    - [ ] 컴포넌트 카드 (CCC0000)
    - [ ] Junction 카드 (CCC0101-0199)
    - [ ] 슬롯 매핑 적용
- [ ] 기본 포맷 검증

**Week 9**: 고급 출력 기능

- [ ] SlotMapper 구현
    - [ ] PLAN.md 슬롯 테이블 적용
    - [ ] 복잡한 매개변수 매핑
- [ ] 출력 미리보기
    - [ ] ExportPreview 컴포넌트
    - [ ] 포맷 검증 결과 표시
- [ ] 파일 다운로드
    - [ ] .inp 파일 생성
    - [ ] 포맷 검증 리포트

#### 완료 기준

- ⏳ 유효한 MARS 입력 덱 생성
- ⏳ 슬롯 매핑 완전 구현
- ⏳ 출력 전 검증 통과
- ⏳ 사용자 확인 가능한 미리보기

---

### Phase 7: UI/UX 완성 (주 10)

**목표**: 사용자 경험 최적화 및 품질 보증

#### 주요 작업

- [ ] 속성 패널
    - [ ] PropertiesPanel 구현
    - [ ] 동적 매개변수 폼
    - [ ] 실시간 업데이트
- [ ] 프로젝트 관리
    - [ ] 저장/불러오기 완성
    - [ ] Undo/Redo 시스템
    - [ ] 자동 저장 기능
- [ ] 사용자 가이드
    - [ ] 키보드 단축키
    - [ ] 컨텍스트 도움말
    - [ ] 튜토리얼 모드
- [ ] 성능 최적화
    - [ ] 대용량 그래프 처리
    - [ ] 메모리 최적화
    - [ ] 렌더링 최적화

#### 완료 기준

- ⏳ 100+ 노드에서 원활한 성능
- ⏳ 직관적인 사용자 인터페이스
- ⏳ 테스트 커버리지 >80%
- ⏳ 프로덕션 배포 준비 완료

---

## ⚠️ 위험 요소 및 완화 전략

### 성능 위험

|위험|영향|완화 전략|
|---|---|---|
|대용량 그래프 렌더링 지연|높음|React Flow 최적화, 가상화|
|실시간 검증 성능 저하|중간|디바운싱, 백그라운드 검증|
|메모리 사용량 증가|중간|메모이제이션, 지연 로딩|

### 데이터 일관성 위험

|위험|영향|완화 전략|
|---|---|---|
|CCC 번호 충돌|높음|중앙집중식 번호 관리|
|상태 동기화 오류|높음|Immutable 상태, 트랜잭션|
|검증 상태 불일치|중간|상태 의존성 명시적 관리|

### 사용자 경험 위험

|위험|영향|완화 전략|
|---|---|---|
|학습 곡선 가파름|중간|단계별 튜토리얼, 컨텍스트 도움말|
|복잡한 매개변수 관리|높음|직관적 폼 디자인, 기본값|
|오류 복구 어려움|높음|강력한 Undo/Redo, 자동 저장|

---

## 🎯 성공 기준

### 기능적 요구사항

- [ ] **완전한 MARS 지원**: 11개 모든 컴포넌트 타입 구현
- [ ] **정확한 검증**: PLAN.md의 모든 규칙 구현
- [ ] **올바른 출력**: 유효한 MARS 입력 덱 생성
- [ ] **직관적 UI**: 비전문가도 사용 가능한 인터페이스

### 비기능적 요구사항

- [ ] **성능**: 100+ 노드 그래프에서 < 2초 응답
- [ ] **신뢰성**: 99%+ 검증 정확도
- [ ] **접근성**: WCAG 2.1 AA 준수
- [ ] **확장성**: 새로운 MARS 컴포넌트 추가 용이

### 품질 기준

- [ ] **테스트 커버리지**: >80% 코드 커버리지
- [ ] **타입 안전성**: 100% TypeScript, strict 모드
- [ ] **코드 품질**: ESLint/Prettier 준수
- [ ] **문서화**: 완전한 API 문서 및 사용 가이드

### 현재 진행 상태

- ✅ Phase 1: 기초 인프라 **완료**
- 🎯 Phase 2: 최소 노드 시스템 준비 (1주)
- ⏳ Phase 3: 고급 연결 시스템 (2주)
- ⏳ Phase 4: 컴포넌트별 상세 구현 (2주)
- ⏳ Phase 5: MARS 검증 엔진 (2주)
- ⏳ Phase 6: MARS 출력 시스템 (2주)
- ⏳ Phase 7: UI/UX 완성 (1주)

**전체 진행률**: 14% (1/7 Phase 완료) **예상 완료 시점**: 10주 후

---

## 🚀 배포 및 운영

### 개발 환경

![](http://localhost:63342/markdownPreview/314627813/)

`[![](http://localhost:63342/markdownPreview/1953207654/commandRunner/runrun.png)](http://localhost:63342/markdownPreview/1953207654/markdown-preview-index-lnubgf99jqsfo2s21aandhlp86.html#)npm install          # 의존성 설치 [![](http://localhost:63342/markdownPreview/1953207654/commandRunner/run.png)](http://localhost:63342/markdownPreview/1953207654/markdown-preview-index-lnubgf99jqsfo2s21aandhlp86.html#)npm run dev          # 개발 서버 시작 [![](http://localhost:63342/markdownPreview/1953207654/commandRunner/run.png)](http://localhost:63342/markdownPreview/1953207654/markdown-preview-index-lnubgf99jqsfo2s21aandhlp86.html#)npm run test         # 테스트 실행 [![](http://localhost:63342/markdownPreview/1953207654/commandRunner/run.png)](http://localhost:63342/markdownPreview/1953207654/markdown-preview-index-lnubgf99jqsfo2s21aandhlp86.html#)npm run build        # 프로덕션 빌드 [![](http://localhost:63342/markdownPreview/1953207654/commandRunner/run.png)](http://localhost:63342/markdownPreview/1953207654/markdown-preview-index-lnubgf99jqsfo2s21aandhlp86.html#)`

### 빌드 출력물

- `dist/`: 정적 파일 (HTML, CSS, JS)
- 단일 페이지 애플리케이션 (SPA)
- CDN 배포 가능

### 브라우저 지원

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

---

## 📚 참고 자료

- [PLAN.md](file:///D:/workspace/comp_node_editor/PLAN.md) - MARS 전처리 GUI 설계 스펙
- [React Flow 공식 문서](https://reactflow.dev/)
- SMART.i 매뉴얼 - MARS 컴포넌트 사양
- 핵공학 열수력 시뮬레이션 가이드

---

## 📞 추가 고려사항

### 확장 가능성

- **플러그인 아키텍처**: 새로운 컴포넌트 타입 추가
- **테마 시스템**: 다크/라이트 모드 지원
- **다국어 지원**: i18n 국제화 준비
- **클라우드 연동**: 프로젝트 클라우드 저장 기능

### 성능 최적화

- **코드 분할**: 컴포넌트별 지연 로딩
- **서비스 워커**: 오프라인 지원
- **웹어셈블리**: 복잡한 계산 최적화
- **CDN 캐싱**: 정적 자원 최적화

이 계획서는 MARS GUI 전처리기의 완전한 재구현을 위한 포괄적인 로드맵을 제공합니다. 각 단계별 구현을 통해 안정적이고 확장 가능한 시스템을 구축할 수 있습니다.