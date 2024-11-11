---
title: Store Table Documentation
description: Description of Table 'Store'
author: jay
date: 2024-11-09 12:00:00 +0800
categories: [FitDine]
tags: [FitDine, table, user]
pin: false
math: true
mermaid: true
---


## Table Name: `diner`

음식점의 기본 정보를 포함한 테이블로, 각 음식점의 소유자와 주소 정보를 관리합니다.

---

### Columns:

| **Column Name**     | **Data Type**  | **Nullable** | **Unique** | **Description** |
|---------------------|----------------|--------------|------------|-----------------|
| `diner_id`          | `BIGINT`       | `NOT NULL`   | `YES`      | 음식점 ID          |
| `owner_user_id`     | `BIGINT`       | `NOT NULL`   | `NO`       | 음식점 보유자 ID      |
| `diner_name`        | `VARCHAR(255)` | `NOT NULL`   | `NO`       | 음식점 이름          |
| `diner_address`     | `VARCHAR(255)` | `NOT NULL`   | `NO`       | 음식점 주소          |
| `delete_yn`         | `VARCHAR(1)`   | `NOT NULL`   | `NO`       | 삭제 여부           |
| `created_user_id`   | `BIGINT`       | `NOT NULL`   | `NO`       | 등록자 ID          |
| `created_datetime`  | `DATETIME`     | `NOT NULL`   | `NO`       | 등록 일시           |
| `modified_user_id`  | `BIGINT`       | `NOT NULL`   | `NO`       | 수정자 ID          |
| `modified_datetime` | `DATETIME`     | `NOT NULL`   | `NO`       | 수정 일시           |

---
### Detailed Description:

- **`diner_id`**:
  - 음식점을 고유하게 식별하는 ID로, 자동 증가하며 테이블의 기본 키입니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `YES`

- **`owner_user_id`**:
  - 음식점 소유주의 ID로, `user` 테이블의 `user_id`를 참조합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`diner_name`**:
  - 음식점명을 나타내며, 최대 255자를 허용합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`diner_address`**:
  - 음식점 주소를 나타내며, 최대 255자를 허용합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`delete_yn`**:
  - 메뉴의 삭제 여부를 나타내며, `'Y'` 또는 `'N'` 값을 가질 수 있습니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`created_user_id`**, **`created_datetime`**, **`modified_user_id`**, **`modified_datetime`**:
  - 각 상품의 생성 및 수정 정보를 기록하는 메타데이터.

---


### Example SQL Query:

```sql
CREATE TABLE `diner`
(
  diner_id           BIGINT           NOT NULL AUTO_INCREMENT COMMENT '음식점 ID',
  owner_user_id      BIGINT           NOT NULL               COMMENT '음식점 보유자',
  diner_name         VARCHAR(255)     NOT NULL               COMMENT '음식점 이름',
  diner_address      VARCHAR(255)     NOT NULL               COMMENT '음식점 주소',
  delete_yn          VARCHAR(1)       NOT NULL DEFAULT 'N'   COMMENT '삭제 여부',
  created_user_id    BIGINT           NOT NULL               COMMENT '등록자 ID',
  created_datetime   DATETIME         NOT NULL               COMMENT '등록 일시',
  modified_user_id   BIGINT           NOT NULL               COMMENT '수정자 ID',
  modified_datetime  DATETIME         NOT NULL               COMMENT '수정 일시',

  PRIMARY KEY (diner_id),
  CONSTRAINT fk_diner FOREIGN KEY (owner_user_id) REFERENCES `user`(user_id)
) ENGINE = INNODB DEFAULT CHARSET=utf8mb4 COMMENT='음식점';
```

---


