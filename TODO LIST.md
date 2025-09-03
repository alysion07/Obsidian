

- [ ] **USB 허브 구매** 
신재호 선임에게 구매 이력 확인 및 관련자료 없을 시, 품의서 작성 
- [UGREEN CM512 (7포트/USB 3.0 Type C) : 다나와 가격비교](https://prod.danawa.com/info/?pcode=20696873)


---

# WBS 
```dataviewjs
// 올해의 연도 가져오기
const year = new Date().getFullYear();

// 1월 1일과 12월 31일의 Date 객체 생성
const jan1 = new Date(year, 0, 1);
const dec31 = new Date(year, 11, 31);

// 1월 1일이 속한 주의 월요일 구하기
const startOfWeek = new Date(jan1);
startOfWeek.setDate(jan1.getDate() - jan1.getDay() + (jan1.getDay() === 0 ? -6 : 1));

// 12월 31일이 속한 주의 월요일 구하기
const lastWeekStart = new Date(dec31);
lastWeekStart.setDate(dec31.getDate() - dec31.getDay() + (dec31.getDay() === 0 ? -6 : 1));
// 마지막 주의 일요일은 그 월요일에 6일을 더한 날
const endOfWeek = new Date(lastWeekStart);
endOfWeek.setDate(lastWeekStart.getDate() + 6);

// 폴더 내의 페이지들 중, 시작일과 마감일이 올해 전체 범위와 겹치는 항목들 선택
const projects = dv.pages('"PROJECT"')
    .where(p => p.enddate && (p.startdate <= endOfWeek && p.enddate >= startOfWeek))
    .sort(p => p.enddate, 'desc');

const mermaidConf = `mermaid
gantt
    dateFormat  D-M-YYYY
    axisFormat %m-%d
    todaymarker on`;

let tasks = "";
projects.forEach(page => {
  const title = page.file.name;
  const startDate = page.startdate ? `${page.startdate.day}-${page.startdate.month}-${page.startdate.year}` : 'unknown';
  const dueDate = page.enddate ? `${page.enddate.day}-${page.enddate.month}-${page.enddate.year}` : 'unknown';
  
  tasks += `    ${title} : ${startDate}, ${dueDate}\n`;
});

const backticks = "```";
dv.paragraph(
  `${backticks}${mermaidConf}
${tasks}
${backticks}`
);


```
---

# TODO 

<iframe src="https://app.projectplan-powerpoint.com/#/?frame=1&src=browser&planId=607f070b-bc6c-4f4f-9735-9291a8953be3&secret=e8090c2c-d300-4233-8b2a-72119022ccb2" style="width: 100%; height: 600px; box-shadow: 0px 0px 14px #ccc"</iframe>

--- 

## 2. 연합트윈 3세부 2차 PoC 개발
전처리기 개발 
	- PoC에 적합한 솔버 선정
		- ***솔버연구팀에 어떤  솔버를 사용할 것 인지 문의 필요*** 
			**문의 사항** 
			1. 반응이 좋았던 솔버는?
			2. 결과 파일의 평균적인 규모. 
				1. 규모에 따라 **Desktop App** or **Web** 결정 가능 
			3. JSON 포맷 확인 (for 인터페이스 설계)
	- 솔버 운용 워크플로우 구성 
	- 전처리 기능 개발 		
		- 기 개발 기능  (**slice** / **geometry**) 포함. 총 **5**개 기능 개발 
	- 후처리기와의 연결성 기획

- 후처리기 개발 
	- LBM 후처리 기능 개발 
	- Data Plotting 기능 개발 
	- Dashboard 기능 개발 
- **오픈소스 작업**   
	- 라이선스 검토 

---
## 3. NDX Cloud Pix4D
- 향후 방향성 수립 필요

---

## 4. NFLOW 개발
- 향후 방향성 수립 필요 
- 
---




### 낮은 우선순위
#### JavaScript
- 1급 객체 
- Event Sourcing Pattern
- 유사배열과 이터러블
- 불변성법칙(immutability)



---
## 일정 산정
- 다음 버전 릴리즈에 맞춰서 일정 고려 
- QA, QC 기간 고려해서 사전에 끝낼 수 있도록 일정 산정
- 자체 R&D의 경우는 당위성을 설정해야 할 것.
##### 프로젝트의 단위 별 상세 항목

>기획서 
>프로젝트별 수행 계획서
>구현 
>테스트

MAN MONTH 기준 기간 산출 하기위한 상세 기획 필요


Rendering Engine 
  - Mouse Contorl 기능 고도화
  - VTK 가시화 기능 개발
 
현대차 지그 유닛 자동 리깅 플러그인
   - 머신 카테고리별 테스트 진행

---


# GPT Review webhook
- Web Hook 설정 방법
- GitLab Login → Project 선택 → Settings → Webhooks
- URL : [http://172.16.28.218:53780/webhook/gitlab](http://172.16.28.218:53780/webhook/gitlab "http://172.16.28.218:53780/webhook/gitlab")
- Name(Optional) : GPT Code Review
- Secret token : ndxpro123!
- Trigger : Merge request events
- SSL Verification : Disable
