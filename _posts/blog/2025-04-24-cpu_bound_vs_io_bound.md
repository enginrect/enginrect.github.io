---
title: CPU bound vs IO bound
description: 연산이 집중적으로 필요한 작업과 디스크/네트워크 등 입출력 대기가 많은 작업
author: jay
date: 2025-04-24 12:00:00 +0800
categories: [Blogging]
tags: [blog, devops, kubernetes]
pin: false
math: true
mermaid: true
---

- CPU Bound: CPU 연산이 주요 병목이 되는 작업. (ex. 암호화, 과학 계산, 비디오 렌더링 등)
- IO Bound: 디스크, 네트워크 등 외부 리소스 접근이 병목이 되는 작업. (ex. DB 호출, 파일 시스템 접근 등)
- k8s에서는 각각의 특성에 맞게 리소스 요청/제한, 스케줄링 정책, 오토스케일링 전략을 통해 다룸.

[공식문서 - Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)


- CPU Bound vs IO Bound는 프로그램 실행 시 병목 위치에 따른 분류
- 이 개념은 쿠버네티스의 리소스 요청/제한 및 오토스케일링 설정에 직접적으로 영향을 줌
- 예를 들어 Docker Compose는 이런 리소스 기반 스케줄링을 제공하지 않음

### 사용이 적합한 환경:
- CPU Bound: 머신러닝 학습, 데이터 처리 파이프라인, 인코딩 서버 등 고연산 환경
- IO Bound: API 서버, 백엔드 서비스, DB 연동 처리 로직 등 대기 시간이 많은 환경


### 쿠버네티스에서...

1. 리소스 요청/제한 설정 (CPU Bound에 중요)
```yaml
resources:
  requests:
    cpu: "2"
  limits:
    cpu: "4"
```
CPU Bound 작업일수록 requests.cpu 값을 높게 설정해야 적절한 노드에 스케줄됨

2. HPA (Horizontal Pod Autoscaler) 설정 (IO Bound에 유리)
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```
IO Bound 작업은 병렬성이 중요하므로 HPA로 처리량 증가를 유도

3. QoS 클래스 활용
- `Guaranteed`, `Burstable`, `BestEffort` 중 CPU Bound에는 Guaranteed가 유리

[공식문서 - Configure Quality of Service for Pods](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/)

4. Node Affinity, Taints & Tolerations 활용
- 고성능 노드에 CPU Bound 작업을 배치하고, IO Bound는 일반 노드로 분산

[공식문서 - Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)