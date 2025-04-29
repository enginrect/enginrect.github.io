---
title: SOAP vs REST
description: 웹 서비스 통신 방식 두 가지인 SOAP와 REST의 차이 비교
author: jay
date: 2025-04-27 12:00:00 +0800
categories: [Blogging]
tags: [blog, web, api, soap, rest]
pin: false
math: false
mermaid: false
---

### 개념 설명:
> **SOAP (Simple Object Access Protocol)**: SOAP Version 1.2 is a lightweight protocol intended for exchanging structured information in a decentralized, distributed environment.   
> [공식문서 (W3C SOAP)](https://www.w3.org/TR/soap/) \
> XML 기반의 메시지 프로토콜로, 높은 수준의 보안과 신뢰성이 필요한 분산 환경에 적합
> 
> **REST (Representational State Transfer)**: 
> REST is an acronym for REpresentational State Transfer and an architectural style for distributed hypermedia systems.  
> [공식문서 (RESTful API)](https://restfulapi.net/) \
> 리소스를 URI로 식별하고 HTTP 메서드(GET, POST 등)를 활용해 조작하는 아키텍처 스타일

### 비슷한 개념과 비교:
- **데이터 포맷**
  - SOAP: XML만 사용
  - REST: JSON, XML, HTML, plain text 등 다양한 포맷 지원
- **상태성(Statefulness)**
  - SOAP: 기본적으로 **stateful**하지만 stateless도 가능
  - REST: **stateless** 설계 권장
- **표준화 수준**
  - SOAP: WSDL(서비스 정의), XSD(데이터 정의) 등 **엄격한 규약**
  - REST: **비공식적이고 유연한 방식**, 표준 메타데이터 없음
- **트랜잭션/보안**
  - SOAP: WS-Security, WS-ReliableMessaging 등 **확장 기능 지원**
  - REST: HTTP 보안(HTTPS, 인증 헤더 등) 위주

### 사용이 적합한 환경:
- **SOAP**:
  - 엔터프라이즈 시스템, 금융/보험 업계
  - 보안/트랜잭션이 중요한 환경 (예: B2B 서비스)
- **REST**:
  - 모바일/웹 애플리케이션
  - 빠른 응답과 경량 통신이 중요한 환경 (예: SNS, 공공 API)

### 사용 방법:
#### SOAP 예시:
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:web="http://enginrect.com/webservice">
   <soapenv:Header/>
   <soapenv:Body>
      <web:getUser>
         <web:id>123</web:id>
      </web:getUser>
   </soapenv:Body>
</soapenv:Envelope>
```

#### REST 예시:
```http
GET /users/123 HTTP/1.1
Host: api.enginrect.com
Accept: application/json
```
