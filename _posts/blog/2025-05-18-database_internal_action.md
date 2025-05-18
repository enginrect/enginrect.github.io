---
title: 데이터베이스의 내부 동작 원리
description: RDBMS(MySQL)와 NoSQL(MongoDB)의 내부 구조와 작동 방식을 비교 정리
author: jay
date: 2025-05-18 10:00:00 +0800
categories: [Blogging]
tags: [blog, database, devops]
pin: false
math: true
mermaid: true
---


### RDBMS - MySQL 기준

### WHAT IS RDBMS?
> 관계형 데이터베이스는 데이터를 행(Row)과 열(Column)로 구성된 테이블(Table)에 저장하며, 이들 간 관계(Relation)를 통해 데이터를 모델링함. \
> SQL(Structured Query Language)을 통해 데이터를 정의하고 조작함.


- **내부 구조 구성요소:**
    - **쿼리 파서(Query Parser)**: SQL 문장을 구문 분석
    - **쿼리 옵티마이저(Query Optimizer)**: 최적 실행 계획 수립
    - **스토리지 엔진(Storage Engine)**: 실제 데이터 저장 처리 (기본: InnoDB)
    - **버퍼 풀(Buffer Pool)**: 자주 쓰이는 페이지를 메모리에 캐싱
    - **트랜잭션 관리**: Redo/Undo 로그를 통한 복구와 Rollback 처리
    - **인덱스**: B+ Tree 기반, 쿼리 성능 최적화

- **사용이 적합한 환경:**
    - 정형화된 데이터 구조가 필요한 **대규모 시스템**
    - ACID 트랜잭션이 중요한 환경 (금융, ERP 등)
    - 복잡한 쿼리 및 조인이 필요한 환경 

#### MySQL vs PostgreSQL
대표적으로 MySQLl과 PostgreSQL이 있음
- **MySQL**: 빠른 읽기 성능, 다양한 스토리지 엔진 지원, 사용이 간편함
- **PostgreSQL**: ACID 준수, JSONB 지원, 복잡한 쿼리 처리에 강함

PostgreSQL은 ACID와 확장성 측면에서 강력하지만, MySQL은 더 빠른 읽기 성능과 쉬운 설정으로 널리 사용됨.

### MySQL 설치 예시 (Docker):
```bash
# 설치
docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=enginrect-secret-pw \
  -e MYSQL_DATABASE=enginrectdb \
  -e MYSQL_USER=enginrect \
  -e MYSQL_PASSWORD=enginrectpw \
  -p 3306:3306 \
  mysql:8.0

# 접속
docker exec -it mysql-container mysql -u root -p
# 입력: enginrectpw
```
---

### NoSQL

### WHAT IS NOSQL?
> NoSQL은 비관계형 데이터베이스의 총칭으로, 테이블 기반이 아닌 다양한 데이터 모델(문서, 키-값, 그래프, 컬럼 등)을 사용. \
> 스키마 유연성, 고성능, 수평 확장을 특징으로 하며, 대규모 분산 시스템에 최적화되어 있음.

- **내부 구조 구성요소:**
    - **문서(Document) 기반 저장**: BSON(Binary JSON) 형식
    - **컬렉션(Collection)**: 테이블에 해당하는 단위
    - **WiredTiger 스토리지 엔진**: 압축, 동시성, 캐시 기능 제공
    - **Write-Ahead Logging (WAL)**: 데이터 손실 방지용
    - **메모리 맵핑**: 데이터파일을 OS 페이지 캐시와 연동
    - **샤딩(Sharding)** 및 복제(Replication) 기능 지원

- **사용이 적합한 환경:**
    - 유동적인 데이터 구조가 필요한 **스타트업/애자일 프로젝트**
    - 빠른 개발/변경이 요구되는 환경
    - 대량의 데이터 분산 저장이 필요한 환경 (IoT, 로그 저장 등)

####  MongoDB vs Cassandra
대표적으로 MongoDB과 Cassandra가 있음
- **MongoDB**: 문서 기반 구조로 유연성 및 개발 생산성 중심
- **Cassandra**: 분산형 데이터베이스로 높은 가용성과 확장성 중심

MongoDB는 문서 기반 구조로 유연성 및 개발 생산성 중심, Cassandra는 고가용성 중심

- **사용 방법 예시:**
  ```bash
  sudo apt install mongodb
  mongosh
  use enginrect_db
  db.users.insertOne({ name: "enginrect", email: "enginrect@gmail.com" })
  db.users.find()
  ```

### MongoDB 설치 예시 (Docker):
```bash
# 설치
docker run -d \
  --name mongodb-container \
  -e MONGO_INITDB_ROOT_USERNAME=enginrect \
  -e MONGO_INITDB_ROOT_PASSWORD=enginrectpw \
  -p 27017:27017 \
  mongo:6.0

# 접속
docker exec -it mongodb-container mongosh -u enginrect -p enginrectpw --authenticationDatabase admin
```
