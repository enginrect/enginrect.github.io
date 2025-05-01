---
title: Kubernetes Secret with Git
description: Kubernetes Secret을 GitOps 환경에서 안전하게 관리하는 방법
author: jay
date: 2025-04-25 12:00:00 +0800
categories: [Blogging]
tags: [blog, kubernetes, gitops, secret, devsecops]
pin: false
math: true
mermaid: true
---


### 개념 설명:
Kubernetes의 `Secret`은 비밀번호, 토큰, 인증 키 같은 민감한 데이터를 저장하고 관리하는 리소스.  
기본적으로 Base64로 인코딩되며, 환경변수, 볼륨, 커맨드라인 인자 등을 통해 Pod에 주입할 수 있음.

> “A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in a container image.”  
> [공식문서](https://kubernetes.io/docs/concepts/configuration/secret/)

하지만 Base64 인코딩은 **암호화가 아니며**, Git에 그대로 올릴 경우 민감 정보 유출 위험이 큼.  
GitOps 환경에서는 이를 안전하게 처리하기 위한 여러 도구가 필요.

---

### 비슷한 개념과 비교:

| 항목 | 목적 | Git 저장 가능 여부 | 보안 수준 |
|------|------|------------------|----------|
| ConfigMap | 비민감한 설정값 저장 | O | 낮음 |
| Secret | 민감 정보 저장 (Base64 인코딩) | X | 낮음 |
| Sealed Secret | 공개키 암호화된 Secret | O | 높음 |
| External Secrets | 외부 비밀 시스템에서 Secret을 동기화 | O | 매우 높음 |
| SOPS | YAML 파일 자체를 암호화 | O | 높음 |

---

### 사용이 적합한 환경:
- GitOps 기반 배포 파이프라인 (예: Argo CD, Flux)
- 보안 정책상 Git 저장이 필요한 조직
- 외부 비밀 관리 시스템(AWS/GCP/Vault 등)을 이미 사용 중인 조직
- 변경 이력 및 감사 로그가 필요한 환경

---

### 사용 방법:

#### 1. Sealed Secrets (Bitnami)
- `kubeseal` CLI로 Secret을 암호화해 Git에 저장
- 클러스터 내 컨트롤러가 SealedSecret을 복호화해 실제 Secret 생성

```bash
kubectl create secret generic my-secret --from-literal=password=1234 --dry-run=client -o yaml > secret.yaml
kubeseal --format=yaml < secret.yaml > sealed-secret.yaml
```

> [Sealed Secrets 공식 문서](https://github.com/bitnami-labs/sealed-secrets)

---

#### 2. External Secrets (ESO)
- AWS Secrets Manager, HashiCorp Vault 등과 연동
- Secret을 Git이 아닌 외부 시스템에 저장하고, 클러스터에서 동기화

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: db-credentials
spec:
  secretStoreRef:
    name: aws-secret-store
    kind: ClusterSecretStore
  target:
    name: db-secret
  data:
    - secretKey: password
      remoteRef:
        key: prod/db/password
```

> [External Secrets 공식 문서](https://external-secrets.io/)

---

#### 3. SOPS + Kustomize
- YAML 파일 자체를 GPG, AWS KMS 등으로 암호화해 Git에 저장
- 배포 시 자동으로 복호화되어 사용됨

```bash
sops -e secret.yaml > secret.enc.yaml
```

- Kustomize와 함께 사용해 GitOps 파이프라인에 통합 가능

> [SOPS 공식 문서](https://github.com/getsops/sops)

---


Kubernetes Secret을 Git에 저장할 때는 다음의 보안 수칙을 따라야 한다:
- Git에는 **절대 평문 Secret을 저장하지 말 것**
- 암호화 도구(Sealed Secrets, SOPS) 또는 외부 비밀 저장소(ESO)를 활용할 것
- GitOps 파이프라인에서 자동화된 보안 흐름을 구축할 것
