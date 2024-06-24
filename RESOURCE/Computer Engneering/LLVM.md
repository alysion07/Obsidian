### 구조 

[참고 링크1](https://velog.io/@thdalwh3867/LLVM-컴파일러의-활용)

컴파일러는 'FrontEnd-MiddleEnd-BackEnd'의 단계로 구성.

보통 이 세 단계는 하나의 프로그램으로 일괄 처리되는데, 이럴 경우 '언어의 종류 x 아키텍처'의 종류 만큼 복수의 컴파일러가 필요하게 됨.
이런 컴파일러의 구조는 재사용성을 떨어뜨린다는 문제가 있음. 이를 해결하기 위한 구조가 **LLVM**이다.

![[vCwbbsffdVOmFcKC-7Tz_JcwmGJDM4LVR_-NOj5qwfp5RcA2V_CpmkSCSHZy23bp2aQALkpi7yGWtG71-E0FYez4YS6v98z_HmbHrE0KXvY3_oA__621pKxFiL1PQGsGF0lIkfFGdnjyZj861NwPzQ.webp]]

LLVM은 아키텍처 별로 분리된 **모듈식 MiddleEnd-BackEnd**를 중점으로 함

1. FrontEnd가 여러가지 프로그래밍 언어들을 **중간 표현 코드**로 번역
2. LLVM은 그 중간 표현 코드를 각각의 아키텍처에 맞게 최적화하여 실행이 가능한 형태로 바꾸는 방식


---

### Clang

Clang은 libclang과 그 FrontEnd로 구성된 [C](https://namu.wiki/w/C%EC%96%B8%EC%96%B4 "C언어") , [C++](https://namu.wiki/w/C%2B%2B "C++"), [Objective-C](https://namu.wiki/w/Objective-C "Objective-C")용 컴파일러이다. '클랭'이라고 읽으며, 2007년에 첫 등장하였다. LLVM 프로젝트의 메인 FrontEnd라고 할 수 있다. [소스 코드](https://namu.wiki/w/%EC%86%8C%EC%8A%A4%20%EC%BD%94%EB%93%9C "소스 코드")를 LLVM IR로 컴파일하는 역할을 담당


---

### LLVM의 작업 흐름

1. **Clang에 의한 AST 생성**: 첫 번째 단계에서는 소스 코드가 Clang 컴파일러 프론트엔드에 의해 처리됩니다. Clang은 C, C++, Objective-C, 그리고 Objective-C++ 등의 언어를 지원합니다. 이 단계에서 소스 코드는 추상 구문 트리(AST)로 변환됩니다. AST는 프로그램의 구조를 트리 형태로 나타내며, 각 노드는 연산자나 문장과 같은 프로그램의 구성 요소를 나타냅니다. 이 트리는 소스 코드의 구문적 구조를 명확하게 표현하므로 컴파일러가 추가 작업을 수행하기 용이한 형태입니다.
    
2. **LLVM IR로의 전환**: AST가 생성된 후, 다음 단계에서는 이를 LLVM의 중간 표현(IR) 형태로 변환합니다. LLVM IR은 높은 수준의 언어와 기계 코드 사이의 중간 단계로, 다양한 최적화를 수행하기에 적합한 형태입니다. LLVM IR은 플랫폼 독립적이며, SSA(Static Single Assignment) 형태로 되어 있어서 각 변수가 한 번만 할당되는 형태를 갖습니다. 이는 컴파일러가 데이터 흐름과 최적화를 더 쉽게 분석할 수 있게 해줍니다.
    
3. **기계 코드로의 변환**: 마지막 단계에서는 LLVM IR을 대상 시스템의 기계 코드로 변환합니다. 이 과정에서는 레지스터 할당, 명령 선택, 명령 스케줄링 등 다양한 하드웨어 최적화가 수행됩니다. LLVM은 다양한 타겟 아키텍처를 지원하므로, 이 단계는 특정 하드웨어의 특성에 맞게 코드를 최적화합니다.