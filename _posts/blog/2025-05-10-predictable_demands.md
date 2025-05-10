---
title: 예측범위 내의 요구사항(Predictable Demands)
description: Kubernetes에서 애플리케이션 자원 요구사항을 선언하는 방법과 그 중요성
author: jay
date: 2025-05-10 12:00:00 +0800
categories: [Blogging]
tags: [blog, kubernetes, devops]
pin: false
math: true
mermaid: true
---

### 개념 설명:
> Kubernetes에서는 애플리케이션이 필요한 리소스를 명확히 선언함으로써 클러스터 스케줄러가 적절한 노드를 선택할 수 있도록 해야 함. \
> 이를 "**Predictable Demands**"라고 하며, 애플리케이션이 자원을 얼마나 소비할지 명확히 예측하고 선언해야만 안정적인 운영이 가능함.  


#### vs. Implicit Demands (암묵적 자원요구)
- 선언하지 않고 실행 시 자원 사용량에 따라 스케줄링되는 방식 → 비효율적이며 예측 어려움


### k8s에서는 어떻게?
- `resources.requests`와 `resources.limits` 필드를 통해 설정
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: predictable-app
spec:
  containers:
  - name: app-container
    image: enginrect.com/predictable-app:latest
    resources:
      requests:
        memory: "512Mi"
        cpu: "500m"
      limits:
        memory: "1Gi"
        cpu: "1"
```

- `requests`: **최소 보장 자원** – 스케줄링 기준
- `limits`: **최대 허용 자원** – 컨테이너 제한치

### 주의할 점:
- `requests`를 선언하지 않으면, K8s는 기본값(0)을 기준으로 스케줄링 → 리소스 과다 사용 위험
- HPA나 VPA 사용 시에도 `requests`가 반드시 필요함
- 자원이 부족한 노드에 할당되는 것을 방지하고자 할 때도 유용

