---
title: Menu Table Documentation
description: Description of Table 'Menu'
author: jay
date: 2024-11-08 12:00:00 +0800
categories: [FitDine]
tags: [FitDine, table, product]
pin: false
math: true
mermaid: true
---


## Table Name: `menu`

메뉴 정보를 포함한 테이블로, 각 메뉴의 기본 정보와 관련된 음식점(Store)과의 관계를 관리합니다.

---

### Columns:

| **Column Name**     | **Data Type**    | **Nullable** | **Unique** | **Description** |
|---------------------|------------------|--------------|------------|-----------------|
| `menu_id`           | `BIGINT`         | `NOT NULL`   | `YES`      | 상품 ID           |
| `store_id`          | `BIGINT`         | `NOT NULL`   | `NO`       | 음식점 ID          |
| `menu_name`         | `VARCHAR(255)`   | `NOT NULL`   | `NO`       | 상품명             |
| `menu_price`        | `DECIMAL(10, 2)` | `NOT NULL`   | `NO`       | 상품 가격           |
| `delete_yn`         | `VARCHAR(1)`     | `NOT NULL`   | `NO`       | 삭제 여부           |
| `created_user_id`   | `BIGINT`         | `NOT NULL`   | `NO`       | 등록자 ID          |
| `created_datetime`  | `DATETIME`       | `NOT NULL`   | `NO`       | 등록 일시           |
| `modified_user_id`  | `BIGINT`         | `NOT NULL`   | `NO`       | 수정자 ID          |
| `modified_datetime` | `DATETIME`       | `NOT NULL`   | `NO`       | 수정 일시           |

---

### Detailed Description:

- **`menu_id`**:
  - 메뉴를 고유하게 식별하는 ID로, 자동 증가하며 테이블의 기본 키입니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `YES`

- **`store_id`**:
  - 메뉴가 속한 음식점의 ID로, User 도메인의`store` 테이블의 `store_id`를 참조합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`menu_name`**:
  - 메뉴명을 나타내며, 최대 255자를 허용합니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`menu_price`**:
  - 메뉴의 가격을 소수점 2자리까지 저장합니다.
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
CREATE TABLE `menu`
(
    menu_id           	BIGINT           NOT NULL AUTO_INCREMENT    COMMENT '메뉴 ID',
    store_id             BIGINT           NOT NULL                   COMMENT '음식점 ID',
    menu_name.           VARCHAR(255)     NOT NULL                   COMMENT '메뉴명',
    menu_price  	        DECIMAL(10, 2)   NOT NULL                   COMMENT  '메뉴 가격',
    delete_yn            VARCHAR(1)       NOT NULL                   COMMENT '삭제 여부',
    created_user_id      BIGINT           NOT NULL                   COMMENT '등록자 ID',
    created_datetime     DATETIME         NOT NULL                   COMMENT '등록 일시',
    modified_user_id     BIGINT           NOT NULL                   COMMENT '수정자 ID',
    modified_datetime    DATETIME         NOT NULL                   COMMENT '수정 일시',

    CONSTRAINT pk_menu PRIMARY KEY (menu_id)
) ENGINE = INNODB DEFAULT CHARSET=utf8mb4 COMMENT='메뉴';
```

---


