---
title: 접근 제어 (Access Control)
description: 시스템 자원에 대한 접근을 통제하는 보안 전략
author: jay
date: 2025-05-14 12:00:00 +0800
categories: [Blogging]
tags: [blog, network, security, aws]
pin: false
math: true
mermaid: true
---


### 개념 설명:
> The process of granting or denying specific requests to  
> 1 obtain and use information and related information processing services and  
> 2 enter specific physical facilities (e.g., federal buildings, military establishments, border crossing entrances). \
> [공식문서 (NIST)](https://csrc.nist.gov/glossary/term/access_control) \
> 정보 및 관련 정보 처리 서비스를 획득하고 사용하는 요청과 특정 물리적 시설(예: 연방 건물, 군사 시설, 국경 출입구 등)에 출입하는 요청에 대해 허용 또는 거부하는 과정.  
 

- 인증(Authentication) vs 인가(Authorization) \
사용자가 누구인지 확인 vs 사용자가 무엇을 할 수 있는지 제어
- RBAC vs ABAC \
역할 기반 vs 속성 기반 접근 제어


- 사용이 적합한 환경:
  - 민감 정보가 포함된 리소스 (S3, RDS, EC2 등)
  - 다양한 사용자/서비스 간의 권한을 세밀하게 관리해야 할 때

### AWS 접근 제어 전략

- **IAM 사용자/역할**: 사용자나 서비스에 권한 부여
- **정책(Policy)**: JSON 형태로 리소스 접근 권한 지정
- **SCP(Service Control Policy)**: AWS Organization에서 계정 단위 제한
- **리소스 정책(Resource Policy)**: S3, Lambda, API Gateway 등에 적용 가능
- **Security Group/NACL**: 네트워크 수준 접근 제한

```json
// S3 버킷 접근 정책 예시 (enginrect 전용)
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReadOnlyAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/ReadOnlyRole"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::enginrect-data/*"
    }
  ]
}
```
- IAM Role `ReadOnlyRole` → S3 `enginrect-data` 버킷의 파일만 조회 가능
- Admin Role은 S3, EC2, RDS 전부 접근 허용
