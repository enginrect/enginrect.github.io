---
title: FitDine TechStck
description: description: This document outlines the technology stack will be used in the project.
author: jay
date: 2024-10-02 12:00:00 +0800
categories: [Blogging]
tags: [FitDine]
pin: false
math: true
mermaid: true
---

# FitDine Project

FitDine 프로젝트는 모바일 어플리케이션으로만 제공한다.

## BackEnd
TechStack : Kotlin, Spring
1. **User**
   사용자 관리를 담당하는 도메인
   회원가입, 로그인, 프로필 관리 등
2. **Order**
   주문 관리 도메인
   주문 생성, 상태 관리 등
3. **Settle**
   정산 관리 도메인
   매출 통계/집계 등
4. **Product**
   음식 메뉴나 재료 관리를 담당하는 도메인
   영양 정보, 재고상태 관리 등
## Frontend
TechStack : Dart, Flutter
1. **Customer**
   사용자(고객) 인터페이스로, 주문하기, 영양소 확인, 목표 설정, 추천 기능 등의 고객 중심 기능 제공
2. **Chef**
   셰프나 음식점 관리자용 인터페이스로, 매출 통계, 재료 관리, 고객 분석 기능 제공
## DevOps
- **GitHub**
  코드 저장소 관리
- **GitHub Actions**
  CI(Continuous Integration) 파이프라인을 구축하여 코드 테스트 및 빌드 자동화
- **ArgoCD**
  CD(Continuous Deployment) 파이프라인을 위한 도구로, Kubernetes 클러스터로 지속적인 배포 자동화
- **AWS**
  클라우드 인프라로서 백엔드와 프론트엔드 서비스를 호스팅
- **Kubernetes(EKS)**
  AWS의 Elastic Kubernetes Service(EKS)를 사용해 컨테이너화된 애플리케이션을 관리하고 오케스트레이션
- Terraform
  IaC(Infrastructure as Code)를 통해 클라우드 리소스를 코드로 관리하고, AWS 인프라 설정 자동화 및 버전 관리.

