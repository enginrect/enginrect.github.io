---
title: SRE가 알고 있어야하는 SLI, SLA, SLO란?
description: 서비스 안정성과 성과 측정을 위한 SRE 핵심 지표 개념 정리
author: jay
date: 2025-05-06 12:00:00 +0800
categories: [Blogging]
tags: [blog, sre, devops, monitoring]
pin: false
math: true
mermaid: true
---

### 개념 설명:
> An **SLI** is a service level indicator—a carefully defined quantitative measure of some aspect of the level of service that is provided. \
> An **SLO** is a service level objective: a target value or range of values for a service level that is measured by an SLI. \
> Finally, **SLA**s are service level agreements: an explicit or implicit contract with your users that includes consequences of meeting (or missing) the SLOs they contain. \
> [공식문서 (Google SRE Book)](https://sre.google/sre-book/service-level-objectives/) \
> SLI (Service Level Indicator)는 시스템의 성능이나 안정성을 측정하는 **정량적 지표**   
> SLO (Service Level Objective)는 SLI에 대해 설정한 **목표 수준**  
> SLA (Service Level Agreement)는 SLO를 기반으로 고객과 맺는 **공식 계약**  




### 예시 구성
- **SLI 예시**: HTTP 요청 성공률
  ```text
  총 요청 수: 1,000,000  
  실패한 요청 수: 500  
  SLI = (1,000,000 - 500) / 1,000,000 = 99.95%
  ```
- **SLO 예시**:
  ```
  enginrect.com의 HTTP 요청 성공률은 30일 간 99.95% 이상이어야 한다.
  ```
- **SLA 예시**:
  ```
  만약 99.95% 미만으로 떨어질 경우, enginrect는 고객에게 월 사용료의 10%를 환불한다.
  ```

### 실무 적용 팁
- 각 서비스마다 별도로 SLI/SLO를 정의
- 에러 버짓(error budget)을 통해 개발 속도와 안정성 간 균형 조절
- SLO 위반 로그 자동화 및 경고 시스템 연동