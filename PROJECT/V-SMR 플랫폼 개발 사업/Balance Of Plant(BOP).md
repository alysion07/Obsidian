# Balance Of Plant(BOP)
## BOP 제어 시스템이란 무엇입니까?

발전소 균형 제어 시스템은 발전소 내의 다양한 보조 시스템을 관리하고 조정하는 포괄적인 제어 메커니즘 네트워크입니다. 이러한 시스템은 펌프, 팬, 열 교환기 및 환경 제어 시스템과 같은 구성 요소의 동기화된 작동을 보장합니다.

주요 목표는 지원 인프라의 효율적이고 안정적이며 안전한 운영을 유지하여 발전소의 전반적인 성능을 최적화하는 것입니다.

## BOP 주요 구성 요소 및 기능

- **환경 제어 시스템:** BOP 제어 시스템은 연도 가스 탈황 시스템, 전기 집진기, 선택적 촉매 환원 시스템을 포함한 ECS 구성 요소를 관리합니다. 이를 통해 배출 및 오염 물질을 제어함으로써 환경 규정을 준수할 수 있습니다.
- **냉각 시스템:** 효율적인 냉각은 발전소 운영에 필수적입니다. BOP 제어 시스템은 냉각탑, 응축기 및 관련 펌프의 기능을 감독하여 최적의 온도를 유지하고 전반적인 열 효율을 향상시킵니다.
- **연료 및 재 처리 시스템:** BOP 시스템은 연료 취급, 연료 저장, 운반 및 준비와 같은 프로세스 관리를 규제합니다. 또한 연소 부산물의 적절한 처리 또는 재사용을 보장하기 위해 재 처리 시스템을 제어합니다.
- **물 처리 및 공급:** 발전소에는 지속적이고 통제된 물 공급이 필요합니다. BOP 제어 시스템은 수처리 공정, 수질 및 분배 시스템을 모니터링하여 다양한 플랜트 기능을 지원합니다.
- **보조 전력 시스템:** 보조 시스템에 안정적이고 신뢰할 수 있는 전원 공급을 보장하는 것이 중요합니다. BOP 제어 시스템은 비상 발전기 및 배터리 백업 시스템을 포함한 보조 전력 시스템을 관리합니다.
- **계측 및 제어 장치:** 이러한 시스템은 다양한 센서, 액추에이터 및 제어 장치를 통합하여 데이터를 수집하고 BOP 구성 요소의 작동을 관리합니다. 여기에는 최적의 조건을 유지하기 위한 제어 루프가 포함됩니다.
## BOP 제어 시스템의 중요성

에너지 산업이 계속 발전함에 따라 BOP 제어 시스템의 역할은 발전의 지속 가능성, 신뢰성 및 최적의 성능을 촉진하는 데 점점 더 중추적인 역할을 하고 있습니다.

### 향상된 효율성

BOP 제어 시스템은 보조 시스템의 성능을 최적화하여 전체 발전소 효율성에 기여하고 에너지 소비를 줄입니다.

### 안전 보장

BOP 제어 시스템 내에 안전 프로토콜을 통합하면 보조 시스템의 안전한 작동을 보장하고 인력, 장비 및 환경을 보호할 수 있습니다.

### 규정 준수 및 환경 관리

규정 준수는 발전소 운영의 중요한 측면입니다. BOP 제어 시스템은 배출 표준을 충족하기 위해 환경 제어 시스템을 관리하는 데 핵심적인 역할을 합니다.

### 운영 신뢰성

BOP 제어 시스템은 보조 구성 요소를 조정하고 모니터링함으로써 발전소 운영의 신뢰성을 향상시키고 가동 중지 시간과 중단을 최소화합니다.



Fuel Receiving,
Storaging Facilities,
Metering System, 
Fuel Oil Storage Tank,
Boiler Proper,
Pulverizer,
Mill,
Fuel Oil Pump,
Burners,
Induced & Forced Draft Fan,
SCR,
Electro-Precipitator,
Ash Handling,
Fuel Gas Desulpherizer,
Stack,
Steam Turbine,
Lube Oil System,
Condenser, 
COP, 
BFP,
Deaerator,
Feedwater Heaters,
Demineralizing Plant,
Cooling Tower,
Closed Cooling Water System,
Circulation Water System, 
Generator Exiter,
Transformers,
Switch Gear and Electrical Facilities,
DCS, CEMS, BMS, APS, 

ETC
Fire Fighting,
HVAC, 
Waste Water Treatment 
Sewer/Drainage System 


복합 화력 발전소
Boiler 대신에Fuel Gas System,
Gas Turbine Generator & Auxiliaries, 
HRSG 등이 설치되어 있고 그 외의 계통은 위의 Fossil Power Plant(기력 발전)와 크게 다르지 않다.



계통도 설계 프로그램

분야별 컴포넌트 구분 
 - Diagram 내에 컴포넌트 삽입 조건 
 - Connection 가능 조건 
 - 컴포넌트 초기값 설정 기능
 - 컴포넌트별 파라미터 입력
	 - 단위 설정
	![[Pasted image 20250122114710.png]]
OMEdit(OpenModelica Connection Editor) 기능 명세서

## 1. 모델링 기능

- Modelica 모델의 그래픽 및 텍스트 기반 생성 및 편집
- Modelica 표준 라이브러리 모델 탐색 및 사용
- 사용자 정의 모델 생성 및 재사용
- 스마트 연결 편집을 통한 모델 인터페이스 간 연결

## 2. 시뮬레이션 기능

- 모델 인스턴스화 및 검사
- 시뮬레이션 실행 및 파라미터 설정
- 변환 디버거 및 알고리즘 디버거 지원
- 실시간 FMU 애니메이션 및 시뮬레이션

## 3. 시각화 및 분석

- 시뮬레이션 결과 변수 플로팅
- 3D 애니메이션 및 시각화
- 그리드 라인 표시/숨김
- 줌 인/아웃 및 리셋 기능

## 4. 사용자 인터페이스

- 라이브러리 브라우저
- 변수 브라우저
- 메시지 창
- 탭/서브윈도우 뷰 전환
- 아이콘/다이어그램/Modelica 텍스트/문서 뷰 제공

## 5. 편집 기능

- 실행 취소/다시 실행
- 클래스 필터링
- 구문 강조 및 자동 완성
- 코드 접기

## 6. 고급 기능

- 다중 프로세서 사용 빌드 옵션
- C/C++ 컴파일러 플래그 설정
- 터미널 명령 실행
- 자동 저장 기능

## 7. 사용자 지정

- 언어 설정
- 작업 디렉토리 설정
- 툴바 아이콘 크기 조정
- 글꼴 및 색상 설정
- 알림 설정

## 8. 상호 운용성

- OMNotebook과의 통합
- FMI(Functional Mockup Interface) 지원

이 기능 명세서는 OMEdit의 주요 기능을 포괄적으로 설명하며, 사용자가 Modelica 모델을 효과적으로 생성, 편집, 시뮬레이션 및 분석할 수 있도록 지원합니다[1][2][5].

Citations:
[1] https://github.com/OpenModelica/OpenModelica-doc/blob/master/UsersGuide/source/omedit.rst
[2] https://openmodelica.org/doc/OpenModelicaUsersGuide/latest/omedit.html
[3] https://openmodelica.org/doc/OpenModelicaUsersGuide/v1.15.0/omedit.html
[4] https://openmodelica.org/doc/OpenModelicaUsersGuide/v1.9.4/omedit.html
[5] https://ep.liu.se/ecp/063/082/ecp11063082.pdf
[6] https://openmodelica.org/doc/OpenModelicaUsersGuide/casella-patch-1/omedit.html
[7] https://www.researchgate.net/figure/An-extension-of-OMEdit-an-Open-Source-graphical-editor-in-OpenModelica-supporting_fig1_262362755
[8] https://script.spoken-tutorial.org/index.php/OpenModelica/C2/Introduction-to-OMEdit/English
[9] https://newsletter.modelica.org/2023-01/index
[10] https://spoken-tutorial.org/watch/OpenModelica/Introduction+to+OMEdit/English/
[11] https://www.informs-sim.org/wsc21papers/206.pdf
[12] https://www.researchgate.net/figure/A-user-interface-of-the-OMEdit-tool_fig5_351306365
[13] https://www.youtube.com/watch?v=FptCa8bplhY
[14] https://github.com/OpenModelica/OpenModelica/issues/9180
[15] https://om.fossee.in/features


SSP 
System Structure and Parameterization