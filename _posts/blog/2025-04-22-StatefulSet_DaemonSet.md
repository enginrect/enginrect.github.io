---
title: StatefulSet vs DaemonSet
description: 
author: jay
date: 2025-04-22 12:00:00 +0800
categories: [Blogging]
tags: [blog, devops, kubernetes]
pin: false
math: true
mermaid: true
---

## StatefulSet vs DaemonSet
---
- StatefulSet은 각 Pod에 고정된 ID, 고정된 스토리지, 순차적 배포가 필요한 워크로드를 위한 리소스.
- DaemonSet은 클러스터의 모든(또는 일부) 노드마다 1개씩 Pod을 배포할 때 사용하는 리소스.
  > (공식문서 링크 - StatefulSet: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
  > (공식문서 링크 - DaemonSet: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
  

- 사용이 적합한 환경:
    - StatefulSet
        - 데이터베이스(MySQL, Cassandra 등)
        - Kafka, Zookeeper처럼 고유 인스턴스가 필요한 시스템
    - DaemonSet
        - 로그 수집기(Fluentd, Filebeat)
        - 노드 모니터링 (Node Exporter, Prometheus Agent)
        - CNI 플러그인, CSI 드라이버 등 모든 노드에 필수 서비스가 필요한 경우

### StatefulSet 간단 예시
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

### DaemonSet 간단 예시
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: log-agent
spec:
  selector:
    matchLabels:
      name: log-agent
  template:
    metadata:
      labels:
        name: log-agent
    spec:
      containers:
        - name: log-agent
          image: fluentd
```
