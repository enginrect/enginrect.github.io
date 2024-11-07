---
title: Order Table Documentation
description: Description of Table 'Order'
author: jay
date: 2024-11-07 12:00:00 +0800
categories: [FitDine]
tags: [FitDine, table, order]
pin: false
math: true
mermaid: true
---


## Table Name: `order`

사용자의 주문 정보를 포함한 테이블로, 주문 내역 및 관련 상품과의 관계를 관리합니다.

---

### Columns:

| **Column Name**     | **Data Type**            | **Nullable** | **Unique** | **Description**        |
|---------------------|--------------------------|--------------|------------|------------------------|
| `order_id`          | `BIGINT`                 | `NOT NULL`   | `YES`      | 주문 ID                |
| `user_id`           | `BIGINT`                 | `NOT NULL`   | `NO`       | 사용자 ID (FK)          |
| `order_number`      | `VARCHAR(255)`           | `NOT NULL`   | `YES`      | 주문 번호 (고유)        |
| `order_date`        | `DATETIME`               | `NOT NULL`   | `NO`       | 주문 일자               |
| `total_amount`      | `DECIMAL(10, 2)`         | `NOT NULL`   | `NO`       | 총 주문 금액           |
| `delete_yn`         | `VARCHAR(1)`             | `NOT NULL`   | `NO`       | 삭제 여부              |
| `created_user_id`   | `BIGINT`                 | `NOT NULL`   | `NO`       | 등록자 ID             |
| `created_datetime`  | `DATETIME`               | `NOT NULL`   | `NO`       | 등록 일시              |
| `modified_user_id`  | `BIGINT`                 | `NOT NULL`   | `NO`       | 수정자 ID             |
| `modified_datetime` | `DATETIME`               | `NOT NULL`   | `NO`       | 수정 일시              |

---

### Detailed Description:

- **`order_id`**:
  - 주문을 고유하게 식별하는 ID로, 자동 증가하며 테이블의 기본 키입니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `YES`

- **`user_id`**:
  - 주문을 생성한 사용자의 ID로, `user` 테이블의 `user_id`를 참조하는 외래 키(FK)입니다.
  - 이를 통해 각 주문이 특정 사용자와 연결됩니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`order_number`**:
  - 주문 고유 번호로, 주문을 고유하게 식별하는 값입니다.
  - `UNIQUE` 제약 조건을 통해 중복되지 않는 값을 유지합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `YES`

- **`order_date`**:
  - 주문이 생성된 날짜 및 시간을 기록합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`total_amount`**:
  - 주문의 총 금액을 저장하며, 소수점 2자리까지 표현 가능합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`delete_yn`**:
  - 주문의 삭제 여부를 나타내며, `'Y'` 또는 `'N'` 값을 가질 수 있습니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`created_user_id`**:
  - 주문을 최초로 생성한 사용자의 ID를 나타냅니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`created_datetime`**:
  - 주문이 최초 생성된 날짜 및 시간입니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`modified_user_id`**:
  - 주문 정보를 최종 수정한 사용자의 ID를 나타냅니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`modified_datetime`**:
  - 주문 정보가 최종 수정된 날짜 및 시간을 기록합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

---

### Example SQL Query:

```sql
CREATE TABLE `order`
(
    order_id               BIGINT           NOT NULL AUTO_INCREMENT    COMMENT '주문 ID',
    user_id                BIGINT           NOT NULL                   COMMENT '사용자 ID (FK)',
    order_number           VARCHAR(255)     NOT NULL UNIQUE            COMMENT '주문 번호',
    order_date             DATETIME         NOT NULL                   COMMENT '주문 일자',
    total_amount           DECIMAL(10, 2)   NOT NULL                   COMMENT '총 주문 금액',
    delete_yn              VARCHAR(1)       NOT NULL                   COMMENT '삭제 여부',
    created_user_id        BIGINT           NOT NULL                   COMMENT '등록자 ID',
    created_datetime       DATETIME         NOT NULL                   COMMENT '등록 일시',
    modified_user_id       BIGINT           NOT NULL                   COMMENT '수정자 ID',
    modified_datetime      DATETIME         NOT NULL                   COMMENT '수정 일시',

    CONSTRAINT pk_order PRIMARY KEY (order_id),
    CONSTRAINT fk_order_user FOREIGN KEY (user_id) REFERENCES `user`(user_id)
) ENGINE = INNODB DEFAULT CHARSET=utf8mb4 COMMENT='주문';
```

---
