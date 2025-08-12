---
목적: " Offline Engine Governmnet Network Env"
주요 범위: 
기대효과: GS인증을 통한 나라장터 진입, 행망 환경에서 Engine 사용
목표 설정: 
담당자 1: matteo.gismondi@pix4d.com
담당자 2: ladislav.palfy@pix4d.com
---
## Checklist 
 - [ ] PIX4D 실무자들과 미팅 예정
 - [ ] Pyc file 암호화 기능 체크 


---

## Thales Sentinel - Docker Container Licensing 
- IT 컨터젼스에서 해줘야 할 작업
- 



## Documents 
Thales Envelope: [Thales Python Envelope.pptx](https://ocoffee-my.sharepoint.com/:p:/g/personal/alysion_e8ight_co_kr/EZU2CYu8_clDqsKej1VFo-EBovJsBwh48OWwBnwWb_4KbA?e=hiNPYW)
### 현재 상황 및 문제점
- 현재 제공되는 PIX4D Engine SDK는 **Online** 전용, **폐쇄망 환경에 공급 불가**한 상황
- PIX4D 본사측에서는 라이선스를 자신들이 **통제할 수 없는 환경에 공급**되는 것을 매우 꺼려하는 상황'
- 이에 당사에서는 [[PYC File Envelope Process|Envelope]] 기능을 활용하여 pyc 파일을 난독화 하는 방법을 제안
	- Thales Sentinel에서 제공하는 암호화 및 난독화 서비스 
	- H/W key 추출 후, python.dll 과 함께 암호화 하여 사용 제한


#### 운용 환경
- 완전히 차단된 폐쇄망(air-gapped)
- 제한적인 G-Cloud networks