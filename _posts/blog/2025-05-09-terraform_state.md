---
title: 테라폼에서 state 관리란?
description: Terraform의 핵심 개념인 state 파일과 그 관리 방식에 대한 이해
author: jay
date: 2025-05-09 12:00:00 +0800
categories: [Blogging]
tags: [blog, devops, terraform]
pin: false
math: true
mermaid: true
---

### 개념 설명:
> Terraform must store state about your managed infrastructure and configuration. This state is used by Terraform to map real world resources to your configuration, keep track of metadata, and to improve performance for large infrastructures. \
> [공식문서](https://developer.hashicorp.com/terraform/language/state)  \
> Terraform은 리소스의 실제 상태를 추적하고 관리하기 위해 state 파일을 사용함.  
> 이 파일은 Terraform이 어떤 리소스를 관리 중인지, 그리고 각 리소스의 속성이 무엇인지를 저장함. \
> 이는 선언형 코드와 실제 인프라 간의 싱크를 유지하는 데 필수적임.

Tip: 협업 및 CI/CD에서의 안정적 상태 추적이 필요한 경우, Remote Backend를 사용하는 것이 좋음.

- 사용 방법:
  - 기본적으로는 `.terraform/terraform.tfstate`에 로컬 저장
  - 위에 언급했듯이 협업 시엔 Remote Backend 사용 권장

### AWS 기반 Remote Backend 구성 예시

#### AWS S3 + DynamoDB Locking
```hcl
terraform {
  backend "s3" {
    bucket         = "enginrect-terraform-state"
    key            = "{keyPrePath}/terraform.tfstate"
    region         = "ap-northeast-2"
    dynamodb_table = "terraform-state-lock"
    encrypt        = true
  }
}
```
- **S3 버킷**: state 파일 저장소
- **DynamoDB 테이블**: 상태 파일 잠금(Locking) 기능 제공

> 이 조합은 팀 단위 협업에 적합하며, 동시에 여러 명이 같은 리소스를 수정하려고 할 때 충돌을 방지.  
[공식문서 - S3 Backend](https://developer.hashicorp.com/terraform/language/settings/backends/s3)  
[공식문서 - State Locking](https://developer.hashicorp.com/terraform/state/locking)

#### IAM 정책

- S3 버킷 이름: `enginrect-terraform-state`
- DynamoDB 테이블 이름: `terraform-state-lock`
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::enginrect-terraform-state/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:DeleteItem"
      ],
      "Resource": "arn:aws:dynamodb:ap-northeast-2:123456789012:table/terraform-state-lock"
    }
  ]
}
```

- 여러 곳에서 state를 관리하면?
  - **문제 발생 가능성**: 충돌, 덮어쓰기, 리소스 꼬임
  - **해결책**:
    - Remote backend 도입
    - Locking 설정 (예: DynamoDB)
    - PR 기반 GitOps 전략으로 배포 분리

!! 하나의 source(state)를 유지하는 것이 가장 중요.
