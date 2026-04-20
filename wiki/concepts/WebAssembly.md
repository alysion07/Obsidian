---
title: WebAssembly
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - WebAssembly
  - 웹
  - 성능
---

# WebAssembly

## 개요

WebAssembly(Wasm)는 JavaScript와 같이 해석(interpret)되는 언어가 아닌, **바이트코드(Byte Code)**를 웹 브라우저로 전송하는 기술이다. CPU 중심 작업을 거의 네이티브(Native) 속도에 가깝게 수행할 수 있다.

## 특징

- **고성능**: 컴파일된 바이트코드를 실행하므로 인터프리터 언어 대비 높은 성능
- **언어 독립적**: C, C++, Rust 등 다양한 언어에서 컴파일 가능
- **브라우저 호환**: 주요 브라우저에서 지원
- **JavaScript 연동**: JavaScript와 상호 운용 가능

## 활용 사례

- CPU 집약적인 연산 (이미지/비디오 처리, 물리 시뮬레이션 등)
- 기존 네이티브 애플리케이션의 웹 포팅
- 게임 엔진, CAD 소프트웨어 등 고성능 웹 애플리케이션

## 관련 문서

- [[JavaScript]] - WebAssembly와 함께 사용되는 웹 언어
- [[Networks]] - 웹 통신 및 네트워크 기초
