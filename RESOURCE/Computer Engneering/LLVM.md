### 구조 

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
