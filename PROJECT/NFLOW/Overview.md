---
Promotion Release: 2025-07-07
Reference URL: "[[THALES Feature]]"
---
## Task List
 - [x] 사용권 계약서 적용 
 - [x] 신규 DMZ IP 적용 (**7/1**, 서버 입고 후 IP 전달 예정)
 - [x] Sentry Transaction 적용 검토 
 - [ ] JSON 키 값을 그냥 int 같은 값으로할까?
 - [ ] 수집된 이슈를 차주에 한번 볼 수 있도록 준비 
 
---



## 프로모션 이후 
- 현재 10명 가량 신청자 
- 수집된 이슈를 차주에 한번 볼 수 있도록 준비 
- 8월 첫 주까지는 다른 방식으로 테스트 진행 후 문서 공유 예정 
- 마이너 업데이트 필요 여부 판단하여 별도의 문서 공유 예정 
- 



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
