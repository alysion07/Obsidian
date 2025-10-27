1개의 격자 20~30cm  파이프는 1M 가량의 격자 사용

#### 노심해석 
제어봉 설계
임계점을 찾는 행위


5만개가량의연료봉을 하나의 셀로 취급하여 푸는 상태

Assembly - 17x17 사이즈의 핵연료봉 
최악의 경우를 모두 상정하여 안전해석 진행 (보수적으로 모의)
평균출력대비 78% 사용하도록 설계
- 사용자의 불확실성 또한 하나의 중요한 팩터

안전해석에 대한검증?


4개의 코드를 연합하여 해석을 진행함
- MARS
- CUPID-RV (MARS 코드와 하나의 코드 처럼 해석 가능)
- MASTER (중성자해석코드)
- PRAPTRAN 핵연료봉 해석 

MARS, CUPID 코드의 실시간성은 어느정도 유사하게 

### MARS 3.1
KAERI 에서 개발한 코드
수 만 라인 까지 길어지는 경우도 있음
Volume 단위 계산

### MARS-KS 
KINS 소유권
NEWSTEP 협약 필요


MARS의 Components

ANNULUS : 구멍이 뚫린파이프

특수 컴포넌트:

junction + volume 합쳐서 999개 제한

Single Junction의 입구출구 구분을 위해 0, 1을 사용하며 
이 값이 유동의 방향을 결정하지는 않는다.

경계조건은 Time dependent junc&vol
pipe는 볼륨을 여러 개를 이어놓은 컴포넌트

MARS는 `*` 가 주석 키워드

MULTID는 9 * 99 * 99 = 88209개의 Node 존재 가능
Input
	Geom
		0: Rectangular
		1: Cylinder


Z가 보통 축 방향

 
Heat Structure ( 핵 연료봉 )
- 기존 규칙성과 다른 규칙을 가지고 있음
	- 기존 C C C C ARD
	- 첫자리가 1이고 C C C C A R D 