---
title: Computer Engineering 개념 모음
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - CS
  - 아키텍처
  - 시스템
---

# Computer Engineering 개념 모음

## Pipeline Architecture (파이프라인 아키텍처)

파이프라인 아키텍처는 여러 명령이 중복 실행되도록 허용하여 성능을 향상시키는 CPU 설계의 핵심 개념이다. 공장의 조립 라인과 유사하게, 다양한 처리 단계(Fetch, Decode, Execute, Memory Access, Write Back)가 동시에 처리된다.

- **이점**: 처리량 증가, CPU 활용도 향상, 확장성
- **과제**: 데이터 위험, 제어 위험, 구조적 위험, 설계 복잡성 증가

## Structure of Arrays (SoA)

SoA(Structure of Arrays)는 데이터 레이아웃 방식 중 하나로, 전통적인 AoS(Array of Structures)와 대비된다. 같은 속성의 데이터를 연속 배열로 저장하여 캐시 효율성과 SIMD 연산 성능을 극대화한다. 게임 엔진, 시뮬레이션, 고성능 컴퓨팅에서 활용된다.

## LLVM

LLVM은 모듈식 컴파일러 인프라로, FrontEnd-MiddleEnd-BackEnd 구조에서 재사용성 문제를 해결한다.

1. **FrontEnd**(예: Clang)가 소스 코드를 AST로 변환 후 LLVM IR(중간 표현)로 전환
2. **MiddleEnd**에서 플랫폼 독립적 최적화 수행 (SSA 형태)
3. **BackEnd**에서 대상 아키텍처의 기계 코드로 변환 (레지스터 할당, 명령 스케줄링 등)

Clang은 C/C++/Objective-C용 컴파일러로 LLVM 프로젝트의 메인 FrontEnd이다.

## ARM (Advanced RISC Machine)

ARM Holdings가 개발한 프로세서 아키텍처로, RISC(Reduced Instruction Set Computing) 기반이다.

- **효율성**: 전력 효율이 뛰어나 모바일, 임베디드, IoT 장치에 이상적
- **다양성**: 스마트폰, 태블릿, 서버 등 폭넓은 디바이스에서 사용
- **명령어 세트**: 단일 주기 실행 가능한 단순화된 명령어 세트 사용
- Thumb/Thumb-2 명령어 세트로 코드 밀도 향상

## PWA (Progressive Web Application)

Native 수준의 기능에 접근하고, 백그라운드에서 실행 가능하며, 앱처럼 작동하는 웹 애플리케이션이다.

1. 독립형 홈 화면 애플리케이션으로 존재하며, 웹 브라우저와 독립적으로 표시
2. 로컬 저장소, Push 알림, Background 실행 지원
3. 자체 웹사이트에서 운영되어 자유로운 수익 창출 가능

## WebWorker

스크립트 연산을 웹 애플리케이션의 주 실행 스레드와 분리된 별도의 백그라운드 스레드에서 실행하는 기술이다.

- UI 프리징 없이 별도의 동작 수행 가능
- 메시지 시스템을 통한 데이터 교환
- 직접 DOM 제어 불가
- **종류**: 공유 워커(SharedWorker), 서비스 워커(Service Worker), 오디오 워커

## 합성곱 (Convolution)

합성곱은 두 함수를 결합하여 새로운 함수를 생성하는 수학적 연산이다. CNN(Convolutional Neural Network)의 핵심 연산으로, 이미지 처리 및 딥러닝에서 특징 추출에 광범위하게 사용된다.

## IA (Information Architecture)

정보 아키텍처는 정보를 체계적으로 구성하고 사용자가 효과적으로 탐색할 수 있도록 설계하는 분야이다. 네 가지 주요 구성 요소:

1. **Organization Systems**: 정보를 분류하고 구성하는 방법 (계층/순차/매트릭스 구조)
2. **Labeling Systems**: 정보를 표현하는 방법
3. **Navigation Systems**: 사용자가 정보를 탐색하거나 이동하는 방법
4. **Searching Systems**: 사용자가 정보를 찾는 방법

## Rust 프로그래밍 언어

Rust는 소유권(Ownership) 시스템을 통해 메모리를 관리하는 시스템 프로그래밍 언어이다.

- **소유권 규칙**: 각 값에는 소유자가 정해져 있고, 동시에 여럿 존재할 수 없으며, 소유자가 스코프를 벗어나면 값이 드롭된다.
- **Stack**: 크기가 정해진 데이터를 LIFO 방식으로 저장, 빠른 접근
- **Heap**: 크기가 가변적인 데이터를 포인터를 통해 저장, 할당/접근이 상대적으로 느림
- 컴파일 타임에 메모리 안전성을 보장하며 실행 속도에 영향을 주지 않음

## Deferred Rendering (디퍼드 렌더링)

Deferred Rendering은 렌더링 파이프라인에서 조명 계산을 지연시키는 기법으로, 먼저 기하학적 정보(위치, 법선, 재질 등)를 G-Buffer에 저장한 뒤, 이후 단계에서 조명을 계산한다. 다수의 광원이 존재하는 장면에서 효율적이다.
