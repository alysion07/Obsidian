---
목적: " Offline Engine Governmnet Network Env"
startdate: 2025-08-11
enddate: 2025-08-20
기대효과: GS인증을 통한 나라장터 진입, 행망 환경에서 Engine 사용
담당자 1: matteo.gismondi@pix4d.com
담당자 2: ladislav.palfy@pix4d.com
---
## Checklist 
 - [ ] pyinstaller 빌드 테스트 
 - [ ] **VM 환경 문의** 
 - [ ] 방화벽 - 특정 포트 개방 필요 여부 
 - [ ] **이원화 시킨 히든 로그의 저장 경로 설정 필요**

---

## 서버 환경
- OS: Linux (계열 조사 필요)
- 가상 머신 환경, 직접 서버에 **USB를 통한 파일 전송은 어려움**(패스스루 지원하지 않나?)
- VPN을 통하여 **SSH 터미널로 접속 가능**, VPN만 되면 **FTP**로 파일 전송 가능
- **pip 패키지 사용 가능** ex) pip install 패키지명
	- 프로그램 구동에 필요한 패키지를 준비해, FTP로 파일 전송 추천 함

---

## 라이선싱 계획(희망편)
1. **Pix4D Offline SDK**의 코드(.pyc)를 Thales LMS의 **Envelope**를 사용하여 난독화 진행 
	- 난독화된 코드는 라이선스 검증이 되어야 정상 작동
2. 1의 결과물과 **'RunScript.py'** 로 구성된 Docker Image를 생성
	- RunScript에는 PGP 사용량 측정을 위해 로그를 작성
	- 로그는 이원화하여 Hidden Log는 별도로 보관 
	- 주기적으로 방문하여 PGP 사용량 측정 후 과금 예정
3. IT컨버젼스 방문하여 호스트 라이선스 매니져 설치
	- **hasp_update** 를 통해 호스트 정보 추출 후 인타이틀먼트(SL)발급
	- ☑️**1947** 포트 방화벽 예외 처리 필요 
	- 같은 대역폭에 있다면 자동으로 호스트의 라이선스 정보를 찾아서 Envelope 동작 


---


## Documents 
Thales Envelope: [Thales Python Envelope.pptx](https://ocoffee-my.sharepoint.com/:p:/g/personal/alysion_e8ight_co_kr/EZU2CYu8_clDqsKej1VFo-EBovJsBwh48OWwBnwWb_4KbA?e=hiNPYW)

## 이전 
- ~~현재 제공되는 PIX4D Engine SDK는 **Online** 전용, **폐쇄망 환경에 공급 불가**한 상황~~
- ~~PIX4D 본사측에서는 라이선스를 자신들이 **통제할 수 없는 환경에 공급**되는 것을 매우 꺼려하는 상황'~~
- ~~이에 당사에서는 [[PYC File Envelope Process|Envelope]] 기능을 활용하여 pyc 파일을 난독화 하는 방법을 제안~~
	- ~~Thales Sentinel에서 제공하는 암호화 및 난독화 서비스~~ 
	- ~~H/W key 추출 후, python.dll 과 함께 암호화 하여 사용 제한~~

---