---
title: Terraform 모듈 의존성
description: Terraform에서 모듈 간의 실행 순서를 제어하는 방법
author: jay
date: 2025-05-08 12:00:00 +0800
categories: [Blogging]
tags: [blog, devops, terraform]
pin: false
math: true
mermaid: true
---
### 개념설명:
> Use the `depends_on` meta-argument to handle hidden resource or module dependencies that Terraform cannot automatically infer.  
> [공식문서](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on) \
> Terraform은 기본적으로 선언형 도구로, 리소스 간 의존성은 자동으로 추론하지만, **모듈 간에는 명시적으로 의존성을 선언해야** 할 때가 있다. `depends_on`을 활용하여 모듈 간의 생성 순서를 지정 가능

- **사용할 수 있는 예시:**
  - VPC 생성 후 해당 서브넷에 EC2 배포
  - RDS 생성 후 인스턴스 연결
  - 여러 인프라 계층(네트워크, 보안, 앱) 모듈로 분리된 구성

- **사용 방법:**
  `vpc` 모듈이 먼저 실행된 후 `ec2` 모듈이 실행되길 원할 때:
  ```hcl
  module "vpc" {
    source = "./modules/vpc"
    ...
  }

  module "ec2" {
    source = "./modules/ec2"
    depends_on = [module.vpc]
    ...
  }
  ```

  output/input을 통해 연결된 경우에도 Terraform은 이를 자동으로 추론하지만,  
  복잡한 인프라에서는 명시적 `depends_on`이 더 명확하고 안전함.

