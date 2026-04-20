---
title: Networks (네트워크)
type: concept
created: 2026-04-14
updated: 2026-04-14
sources:
  - sources/2026-04-14-RESOURCE.md
tags:
  - 네트워크
  - HTTP
  - TCP/IP
  - OSI
---

# Networks (네트워크)

## 개요

네트워크는 컴퓨터와 장치들이 데이터를 주고받기 위해 연결된 시스템이다. 프로토콜, 계층 모델, 웹 통신의 기본 개념을 포함한다.

## Protocol (프로토콜)

네트워크 통신을 위한 규약. 대표적인 프로토콜:

- **TCP/IP**: 인터넷의 기본 통신 프로토콜
- **IP (Internet Protocol)**: 인터넷 주소 체계 및 라우팅
- **HTTP (HyperText Transfer Protocol)**: 웹 통신 프로토콜
- **FTP (File Transfer Protocol)**: 파일 전송 프로토콜

## OSI 7 Layer

OSI 모델은 네트워크 통신을 7개 계층으로 분리한 참조 모델이다.

| 계층 | 이름 | 설명 |
|------|------|------|
| 7 | Application | 사용자 인터페이스 (HTTP, FTP 등) |
| 6 | Presentation | 데이터 표현, 암호화 |
| 5 | Session | 세션 관리 |
| 4 | Transport | 전송 제어 (TCP, UDP) |
| 3 | Network | 라우팅, IP 주소 |
| 2 | Data Link | NIC, MAC 주소, 프레임 |
| 1 | Physical | 물리적 전송 매체 |

2계층(Data Link Layer)에는 NIC(Network Interface Card)가 해당된다.

## TCP/IP

TCP/IP는 인터넷에서 사용되는 통신 프로토콜 모음으로, OSI 모델을 4개 계층으로 단순화한 실용적 모델이다.

## 웹의 동작 방식

1. 사용자가 웹 브라우저에 URL 주소를 입력
2. 도메인 네임을 DNS 서버에서 검색
3. DNS 서버에서 해당 도메인에 대한 IP 주소를 반환
4. HTTP 프로토콜을 사용하여 HTTP 요청 메시지 생성
5. TCP 프로토콜을 사용하여 해당 IP의 웹서버로 전송
6. 웹 서버가 요청에 해당하는 데이터를 검색
7. HTTP 응답 메시지를 생성하여 TCP를 통해 클라이언트로 전송
8. 브라우저가 응답 데이터를 렌더링하여 사용자에게 표시

## HTTP

HTTP는 TCP/IP를 기반으로 하는 애플리케이션 수준 프로토콜이다.

### HTTP 응답 코드

| 범위 | 분류 | 주요 코드 |
|------|------|-----------|
| **1xx** | 정보 응답 | 100 Continue, 101 Switching Protocols |
| **2xx** | 성공 | 200 OK, 201 Created |
| **3xx** | 리다이렉션 | 301 Moved Permanently, 302 Found, 304 Not Modified |
| **4xx** | 클라이언트 오류 | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| **5xx** | 서버 오류 | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

## CORS (Cross-Origin Resource Sharing)

CORS는 다른 출처(Origin)의 리소스에 대한 접근을 제어하는 보안 메커니즘이다.

### Origin의 구성

Origin은 **Protocol + Host + Port**의 조합이다.

### SOP (Same-Origin Policy)

동일한 출처에서만 리소스를 공유할 수 있다는 브라우저 보안 정책이다. SOP가 없으면 CSRF나 XSS 등의 공격에 취약해진다.

### CORS 동작 원리

1. 클라이언트가 HTTP 요청 헤더에 `Origin`을 담아 전달
2. 서버가 응답 헤더에 `Access-Control-Allow-Origin`을 담아 반환
3. 브라우저가 Origin과 Access-Control-Allow-Origin을 비교하여 차단 여부 결정

> 출처 비교와 차단은 **브라우저**가 수행한다. 서버 간 통신에서는 CORS 정책이 적용되지 않으며, 이를 이용한 것이 프록시(Proxy) 서버이다.

### CORS 3가지 시나리오

- **예비 요청 (Preflight Request)**: OPTIONS 메서드로 사전 확인
- **단순 요청 (Simple Request)**: 조건을 만족하면 예비 요청 없이 바로 요청
- **인증된 요청 (Credential Request)**: 쿠키나 토큰 등 인증 데이터를 포함한 요청

### CORS 해결 방법

서버에서 `Access-Control-Allow-Origin` 헤더에 허용할 출처를 기재하여 응답하면 된다. 즉 Back-end 영역의 설정이 필요하다.

## 관련 문서

- [[WebAssembly]] - 웹 기반 바이트코드 기술
