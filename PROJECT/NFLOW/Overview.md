---
Promotion Release: 2025-07-07
Reference URL: "[[THALES Feature]]"
---
## Task List
 - [ ] 경북대학교 ( 금주 금요일 혹은 차주)
 
---

# NFLOW AI-TFT 

## Workflow
해석(다양한 케이스에 대한 해석 사전 진행)
-> Sampling(범위, Method) 설정
-> Target(Fields , section)설정
-> 2D VTK 추출 



## NFLOW 납품 
- FEATURE ID 기준으로 라이선스 수집 여부 결정 하도록 수정 
- **경북대학교 ( 금주 금요일 혹은 차주 )** 
	- **우선 로그 작성 중지하고 추후에 업데이트 전달(로컬 로그 저장 버전)** 
- 현대차 8월 말 (로그 제거 필요, 솔버 업데이트, EULA 교체)

미니 덤프로 생성된 데이터를 사용자가 보낼 수 있는지?
- `attachment` 기능 활용하여 생성된 덤프 파일 보낼 수 있을듯 [초기화 링크](https://docs.sentry.io/platforms/native/enriching-events/attachments/#sdk-initialization)
[[Sentry - Attachment 활용]]


---

## 7.14 사업부 교류회 요약 
 - 프로모션 신청 현황 (학교 7건/ 기업 3건, 총 10건)  
- Sentry로 수집된 이슈 차주에 볼 수 있도록 준비 (solver, 이명연 팀장)  
- 8월 첫 주, 추가 테스트 결과 및 마이너 업데이트 문서 공유 예정   
- 전처리 추가 필요 기능인 '격자 미리보기' 개발 논의 
    - 격자 미리보기 기능명세 후, 전달(후처리 보다 우선 전달 예정)

---
## 프로모션 진행 (7/7)
- 60일 동안의 무료 사용 프로모션 진행  
- 모든 솔버 사용 가능. 기능 제약 없음
- 클라우드 라이선스 방식 사용 
- 사용권 계약서 한 장으로 업데이트 (현재 법무팀에서 검토중)



##### 프로모션 이후 (7/7 ~ )
*NFLOW 4.0* 준비 및 *POST Processor*에 대해 언급됨(target : 내년 1Q )
우선 기획서 작성이 우선이며, 기획서 기반으로 개발 일정 산출 가능하다는 답변 전달(to 사업부)

##### 2Y ~3Y 이후
NFLOW SaaS 구축에 대한 준비 필요
 - PRE/POST는 로컬에서 진행 
 - Full Cloud를 목표로 함

---
## 로그 작성 포맷

기존 LOG_DEBUG에서는 captureMessage 삭제 
새로 매크로 작성해서 addBreadcrumb (Log Level:INFO) 추가 
### GUI
#### breadcrumb 
1. 다이얼로그 Open 
2. 개별 에디터 조작 (Tree 아이템 선택 등)
#### captureMessage
 1. 다이얼로그 Close
 2. 리본 메뉴 버튼 클릭 
#### captureError/Exception (StackTrace 사용)
1. Try/Catch 처리 
2. 각종 에러 상황에서 사용

### Solver/View 로그
1. View/Sovler 쪽 req / res / 예외 처리는 점진적으로 진행
2. Try/Catch 처리 우선 

---
## License Feature
- [라이선스_등급별_기능_정리.pdf](https://ocoffee-my.sharepoint.com/:b:/g/personal/minsu_kim_e8ight_co_kr/EUkR1eR8HxxCvJf4KpfBFP0BvXMlBzme3avonL_HmfXl4g?e=kKMJlV)

![[Pasted image 20250617122637.png| Feature 기능 정의]]
