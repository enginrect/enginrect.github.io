---
title: User Table Documentation
description: Description of Table 'User'
author: jay
date: 2024-10-06 12:00:00 +0800
categories: [FitDine]
tags: [FitDine, table]
pin: false
math: true
mermaid: true
---

## Table Name: `user`

사용자의 로그인정보 및 개인정보를 포함한 테이블.

---

### Columns:

| **Column Name**   | **Data Type**            | **Nullable** | **Unique** | **Description** |
|-------------------|--------------------------|--------------|------------|-----------------|
| `user_id`         | `BIGINT`                 | `NOT NULL`   | `YES`      | 사용자 ID          |
| `email`           | `VARCHAR(255)`           | `NOT NULL`   | `YES`      | 이메일 (로그인)       |
| `password`        | `VARCHAR(255)`           | `NOT NULL`   | `NO`       | 비밀번호            |
| `name`            | `VARCHAR(100)`           | `NOT NULL`   | `NO`       | 이름              |
| `age`             | `INT`                    | `NULL`       | `NO`       | 나이              |
| `gender`          | `ENUM('Male', 'Female')` | `NULL`       | `NO`       | 성별              |
| `mobile`          | `VARCHAR(15)`            | `NULL`       | `NO`       | 휴대폰번호           |
| `signup_datetime` | `DATETIME`               | `NOT NULL`   | `NO`       | 가입일자            |

---

### Detailed Description:

- **`user_id`**:
   - 시스템 내에서 각 사용자를 고유하게 식별하는 ID.
   - 이 열은 자동 증가하며 테이블의 기본 키로 작동합니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `YES`

- **`email`**:
   - 사용자의 이메일 주소로, 주된 연락 수단이자 로그인에 사용됩니다.
   - `UNIQUE` 제약 조건으로 인해 동일한 이메일로 두 명의 사용자가 등록할 수 없습니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `YES`

- **`password`**:
   - 사용자의 비밀번호는 보안상의 이유로 해시된 형태로 저장됩니다.
   - 평문으로 저장되지 않으며, bcrypt와 같은 적절한 해싱 알고리즘을 사용하여 저장해야 합니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `NO`

- **`name`**:
   - 사용자의 전체 이름을 저장합니다. 이 필드는 필수이며 최대 100자까지 입력할 수 있습니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `NO`

- **`age`**:
   - 사용자의 나이를 나타냅니다. 이 필드는 선택 사항이며 사용자의 나이를 정수 값으로 저장합니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `NO`

- **`gender`**:
   - 사용자의 성별을 저장하며, ENUM 타입으로 `'Male'`, `'Female'` 2가지 값 중 하나를 가집니다.
   - 이를 통해 데이터베이스에 유효한 성별 값만 저장될 수 있도록 보장합니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `NO`

- **`mobile`**:
   - 사용자의 휴대폰 번호를 저장합니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `YES`

- **`signup_date`**:
   - 사용자가 등록한 날짜를 저장합니다.
   - 사용자가 생성될 때 기본적으로 현재 타임스탬프가 입력됩니다.
   - **Nullable**: `NOT NULL`
   - **Unique**: `NO`
---

### Example SQL Query:

```sql
CREATE TABLE `user`
(
    user_id                   BIGINT                   NOT NULL  AUTO_INCREMENT  COMMENT '사용자ID',
    email                     VARCHAR(255)             NOT NULL  UNIQUE          COMMENT '이메일',
    password                  VARCHAR(255)             NOT NULL                  COMMENT '비밀번호',
    name                      VARCHAR(100)             NOT NULL                  COMMENT '이름',
    age                       INT                      NOT NULL                  COMMENT '나이',
    gender                    ENUM('MALE', 'FEMALE')   NOT NULL                  COMMENT '성별',
    mobile                    VARCHAR(20)              NOT NULL  UNIQUE          COMMENT '휴대폰번호',
    signup_date               DATE                     NOT NULL                  COMMENT '가입일자',
    delete_yn                 varchar(1)               NOT NULL                  COMMENT '삭제여부',
    created_user_id           BIGINT                   NOT NULL                  COMMENT '등록자ID',
    created_datetime          DATETIME                 NOT NULL                  COMMENT '등록일시',
    modified_user_id          BIGINT                   NOT NULL                  COMMENT '수정자ID',
    modified_datetime         DATETIME                 NOT NULL                  COMMENT '수정일시',

    CONSTRAINT pk_user PRIMARY KEY (user_id)
) ENGINE = INNODB DEFAULT CHARSET=utf8mb4 COMMENT='사용자';




