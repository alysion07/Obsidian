## 1. 목적
- SMART.i 입력 덱을 기준으로 실제 사용된 계통과 파라미터를 정리.    
- 웹 기반 전처리기(React Flow 기반)에서 **노드 생성/명명 규칙**과 **연결선 규칙**을 명확히 정의.    
- 사용자는 그래픽 UI에서 노드와 연결선만 조작.    
- 시스템은 MARS 입력 형식(CCC 카드 구조, from/to, 파라미터)을 자동 생성.    

---

## 2. 컴포넌트 종류 (SMART.i 기준)

SMART.i에서 확인된 주요 계통(컴포넌트 타입):

- **snglvol**: 단일 체적    
- **pipe**: 1D 파이프    
- **sngljun**: 단일 접합    
- **mtpljun**: 다중 접합    
- **branch**: 분기    
- **pump**: 펌프    
- **valve**: 밸브 (트립 연동)    
- **prizer**: 가압기    
- **sg**: 증기발생기 (1차·2차)    
- **separator/dryer**: 증기 분리기/건조기    
- **accumulator**: 축압기    
- **eccmixer**: ECC 혼합기    
- **circltr**: 보조계통 순환기    
- **tmdpvol / tmdpjun**: 시간의존 체적/접합    

---

## 3. 컴포넌트 파라미터 (요약)

### 공통 입력
- **geometry**: x-area, x-length, volume, angle, dz, x-wall, xhd    
- **junction**: from/to, area, forward loss, reverse loss    
- **flags**: jefvcahs (junction control flags)    
- **initial condition**: ebt, press, temp, flow, mfl, mfv    

### 주요 플래그(jefvcahs)

- `e`: PV term    
- `f`: CCFL    
- `v`: stratification    
- `c`: choking    
- `a`: 면적 변화 모델 (0=smooth, 1=full abrupt, 2=partial abrupt)    
- `h`: homogeneity    
- `s`: momentum flux    

### 특수 계통

- **Pump**: inlet/outlet junction, 초기유량, 특성곡선(H-Q, B-Q, etc.)
- **Valve**: open %, Cv, fail position, trip 연동 로직    
- **ECC Mixer**: 주입·정상입구·출구 3점 연결 필수    
- **Accumulator**: 초기 압력, 보론 농도, 분사 조건    

---

## 4. 노드 명명 규칙

- **내부 ID**: UUID (React Flow 관리용)    
- **표시명**: `SYS-KIND-idx` (예: `PRI-PIPE-12`)    
- **내부 번호(CCC)**: 자동 할당, 사용자 편집 불가    
    - Primary: 100–199        
    - Secondary: 200–299        
    - Auxiliary: 300–399        
    - Control: 400–499        
    - 오프셋: pipe:+0, sngljun:+10, mtpljun:+20, pump:+80, valve:+90 …        
- **익스포트 시**: `CCC0000 <name> <type>` 형태로 출력    

---

## 5. 연결선 규칙

### 5.1 from/to 코드

- 형식: `CCCVV000N`    
    - `CCC`: 컴포넌트 번호        
    - `VV`: 체적 번호(볼륨/셀 index)        
    - `N`: 면 번호 (1=in, 2=out, 3~6=crossflow)        

### 5.2 방향성

- **원칙**: `out → in`만 허용    
- **특례**: ECC mixer 등 다중 입력 구조 허용    
- **phase**: liquid ↔ liquid, gas ↔ gas, mix→liquid/gas 허용    
- **capacity**: 포트별 연결 제한 (예: inlet=1, outlet=다중 가능)    

### 5.3 Junction 규칙

- **sngljun**: from/to 필수, area, kfor, krev, jefvcahs 입력    
- **mtpljun**: no of jun, 각 분기별 from/to/area/loss/flags 필요    
- **branch**: njuns 지정, 각 분기별 from/to 지정

### 5.4 Pump 연결

- 반드시 inlet·outlet 존재    
- from=흡입측, to=토출측    
- 손실계수, jefvcahs 입력    
- pump curve 반드시 존재    

### 5.5 Valve 연결

- from/to 지정    
- 개폐 특성 (open%, Cv, failPos)    
- trip logic 연동 (`cntrlvar`, `timeof` 등)    

### 5.6 ECC Mixer

- 3 junction mandatory: ECC 주입, 정상입구, 정상출구    
- 각 포트 지정 (CCC010001=in, CCC010002=out)    

---

## 6. 검증 규칙

- **번호 충돌**: CCC 고유해야 함    
- **포트 충돌**: `(CCC,VV,N)` 중복 불가    
- **방향**: out→in 이외 금지    
- **phase mismatch**: 불가 (예: liquid→gas)    
- **capacity 초과**: 연결 제한 초과 시 금지    
- **금지 조합**: 매뉴얼 정의 (예: accumulator→separator 직접연결 금지)    

---

## 7. 데이터 스키마 (요약)

```ts
type Node = {   
 id: UUID;   
 kind: 'pipe'|'sngljun'|'mtpljun'|'pump'|'valve'|...;  
 sys: 'pri'|'sec'|'aux'|'ctrl';   
 name: string;   
 ccc?: number; // 익스포트 시 부여 
 ports: Port[];   
 meta: {...}; // geom, flags, init cond. 
 };  
 
 type Edge = {   
 id: UUID;   
 source: UUID;   
 sourcePortId: PortId;   
 target: UUID;   
 targetPortId: PortId;   
 mars: {     
   type: 'sngljun'|'mtpljun'|'pump'|'valve'|...;     
   from?: number; // CCCVV000N (익스포트 시)     
 to?: number;     
 area?: number;     
 kfor?: number;     
 krev?: number;     
 flags?: string;
 };
 };
```

---

## 8. 익스포트 규칙

- **노드** → `CCC0000 name type` + 세부 geometry/initial condition 카드 출력    
- **엣지** → `CCC01NN from to area kfor krev flags` 형식으로 출력    
- **자동 변환**: UUID/포트 → CCCVV000N 변환    

---

## 9. UI/UX 지침

- 노드 생성 시 번호 자동 할당, 사용자는 명칭만 확인    
- 연결선 드래그 시: 유효 포트만 하이라이트, 불가 사유 툴팁    
- 연결 직후 팝오버: area, kfor, krev, flags 입력 가능    
- 저장 전 전역 검증: 번호 충돌, 고아 포트, phase mismatch 리포트    

---
# ✅ 결론
- SMART.i 에서 사용된 계통과 파라미터를 기준으로 **노드/엣지 스키마, 명명 규칙, 연결 규칙, 검증 규칙**을 정리    
- 이 문서는 **웹 기반 전처리기 개발의 기초 설계 스펙**으로 활용 가능
- 사용자는 노드/엣지만 다루고, 시스템이 MARS 입력 형식과 규칙을 자동으로 보장한다.