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
    .where(p => p.duedate && (p.startdate <= endOfWeek && p.duedate >= startOfWeek))
    .sort(p => p.duedate, 'desc');

const mermaidConf = `mermaid
gantt
    dateFormat  D-M-YYYY
    axisFormat %m-%d
    todaymarker on`;

let tasks = "";
projects.forEach(page => {
  const title = page.file.name;
  const startDate = page.startdate ? `${page.startdate.day}-${page.startdate.month}-${page.startdate.year}` : 'unknown';
  const dueDate = page.duedate ? `${page.duedate.day}-${page.duedate.month}-${page.duedate.year}` : 'unknown';
  
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


PIX4D FLOW CHART 
python 312
ubuntu 24
engine 2.14


- [ ] 빌드 후 PDB 파일 업로드 할 수 있도록 빌드봇 설정 
- [ ] 외부에서 로그 레벨을 설정할 수 있는지 확인
- [ ] 외부로부터 로그를 전송 받기 위한 포트 설정 요청(to 신재호)





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
