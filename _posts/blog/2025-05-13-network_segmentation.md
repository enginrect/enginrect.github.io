---
title: 네트워크 분리 (망분리)
description: 네트워크를 내부/외부망으로 나눠 보안을 강화하는 전략
author: jay
date: 2025-05-13 12:00:00 +0800
categories: [Blogging]
tags: [blog, security, network, aws]
pin: false
math: true
mermaid: true
---

### 개념 설명:
망분리는 보안성과 안정성을 위해 외부 인터넷망과 내부 업무망을 분리하는 정책.  
일반적으로 물리적 분리(하드웨어적 분리)와 논리적 분리(VLAN, 라우팅 등)로 나뉨.


VLAN vs 물리적 분리: VLAN은 논리적이며 비용 효율적이지만 당연히 보안성은 물리적 분리에 미치지 못함

- 사용이 적합한 환경:
  - 보안이 최우선인 조직 (예: 금융권, 공공기관)
  - 인터넷과 연결된 외부망의 위협을 내부 시스템으로부터 격리하고자 하는 경우



### AWS에서의 망 분리 전략

- **VPC**: Virtual Private Cloud로, 사용자 정의 네트워크를 만들 수 있음
- **Subnet 분리**:
    - Public Subnet: 인터넷 게이트웨이 연결 → 외부와 통신 가능
    - Private Subnet: NAT Gateway 또는 VPN으로만 외부 통신 허용
- **NACL(Network ACL)**: 서브넷 단위의 트래픽 필터링
- **Security Group**: 인스턴스 단위로 인바운드/아웃바운드 트래픽 제어

```bash
# VPC 생성 예시 (AWS CLI)
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Public Subnet, Private Subnet 나누기
aws ec2 create-subnet --vpc-id vpc-xxxx --cidr-block 10.0.1.0/24 --availability-zone ap-northeast-2a
aws ec2 create-subnet --vpc-id vpc-xxxx --cidr-block 10.0.2.0/24 --availability-zone ap-northeast-2a
```

예시:
- public-subnet: 웹 서버 (Nginx) 배포
- private-subnet: DB 서버 배치 (MySQL)
- bastion host: `bastion.enginrect.com` → SSH 접속용
