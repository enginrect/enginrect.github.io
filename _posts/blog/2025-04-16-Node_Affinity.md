---
title: NodeAffinity
description: 어떤 노드에 스케줄될 수 있는지를 노드의 라벨을 기준으로 제어할 수 있게 해주는 기능
author: jay
date: 2025-04-16 12:00:00 +0800
categories: [Blogging]
tags: [blog, devops, kubernetes]
pin: false
math: true
mermaid: true
---

## 🧭 Node Affinity란?

> Node affinity is conceptually similar to `nodeSelector` but allows you to specify rules using `label selectors`. It allows you to constrain which nodes your pod is eligible to be scheduled based on labels on nodes.  
> — from [Kubernetes 공식 문서](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)

Node Affinity는 Pod가 어떤 노드에 스케줄될 수 있는지를 **노드의 라벨을 기준으로 제어**할 수 있게 해주는 기능이다.  
`nodeSelector`보다 더 유연하고 표현력이 뛰어난 방식이다.

---

## 🔍 어떤 상황에서 설정하는가?

- 특정 워크로드를 **GPU가 있는 노드**에만 배치하고 싶을 때
- 특정 팀의 작업을 **지정된 노드 그룹**에서만 실행하고 싶을 때
- **지리적 위치(region, zone)** 라벨 기준으로 워크로드를 배치하고 싶을 때

예시 :
```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/e2e-az-name
          operator: In
          values:
          - e2e-az1
          - e2e-az2
```

| 항목     | nodeSelector | nodeAffinity                                       |
|--------|--------------|----------------------------------------------------|
| 표현력    | 낮음           | 높음 (AND/OR 조합 가능)                                  |
| 연산자    | ==만 지원       | In, NotIn, Exists, DoesNotExist 등 지원               |
| 선호도 설정 | 불가능          | preferredDuringSchedulingIgnoredDuringExecution 가능 |

---
### 사용 가능한 환경
- MSA 환경에서 팀/서비스별 자원 격리
- GPU, SSD 등 특정 HW 리소스를 가진 노드에만 워크로드 배치
- 클러스터에 다양한 노드 타입이 존재하는 하이브리드 환경
---

### 사용법

- requiredDuringSchedulingIgnoredDuringExecution: 필수 조건, 만족하지 않으면 스케줄링 안 됨
- preferredDuringSchedulingIgnoredDuringExecution: 가능한 조건, 만족하면 더 선호

공식 문서 예시:
https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity