---
title: Store_Schedule Table Documentation
description: Description of Table 'Store_Schedule'
author: jay
date: 2024-11-10 12:00:00 +0800
categories: [FitDine]
tags: [FitDine, table, user]
pin: false
math: true
mermaid: true
---


## Table Name: `store_schedule`

음식점의 요일별 운영 시간대를 관리하는 테이블로, 각 음식점의 요일별 오픈 및 종료 시간을 설정합니다.

---

### Columns:

| **Column Name**     | **Data Type** | **Nullable** | **Unique** | **Description** |
|---------------------|---------------|--------------|------------|-----------------|
| `store_schedule_id` | `BIGINT`      | `NOT NULL`   | `YES`      | 음식점 스케줄 ID      |
| `store_id`          | `BIGINT`      | `NOT NULL`   | `NO`       | 음식점 ID (FK)     |
| `day_of_week`       | `ENUM(...)`   | `NOT NULL`   | `NO`       | 요일              |
| `open_time`         | `TIME`        | `NOT NULL`   | `NO`       | 오픈 시간           |
| `close_time`        | `TIME`        | `NOT NULL`   | `NO`       | 종료 시간           |
| `delete_yn`         | `VARCHAR(1)`  | `NOT NULL`   | `NO`       | 삭제 여부           |
| `created_user_id`   | `BIGINT`      | `NOT NULL`   | `NO`       | 등록자 ID          |
| `created_datetime`  | `DATETIME`    | `NOT NULL`   | `NO`       | 등록 일시           |
| `modified_user_id`  | `BIGINT`      | `NOT NULL`   | `NO`       | 수정자 ID          |
| `modified_datetime` | `DATETIME`    | `NOT NULL`   | `NO`       | 수정 일시           |

---
### Detailed Description:

- **`store_schedule_id`**:
  - 음식점 스케줄 ID로 이 테이블의 PK 입니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `YES`

- **`store_id`**:
  - 음식점 ID로, `store` 테이블의 `store_id`를 참조하는 외래 키(FK)입니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`day_of_week`**:
  - 음식점 오픈 요일을 나타내며, MON, TUE, WED, THU, FRI, SAT, SUN 중 하나 입니다.
  - ENUM 으로 관리됩니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`open_time`**:
  - 음식점 오픈 시간을 나타내며, HH:mm:ss 로만 나타냅니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`delete_yn`**:
  - 주문-상품 관계의 삭제 여부를 나타내며, `'Y'` 또는 `'N'` 값을 가질 수 있습니다.
  - **Nullable**: `NOT NULL`
  - **Unique**: `NO`

- **`created_user_id`**, **`created_datetime`**, **`modified_user_id`**, **`modified_datetime`**:
  - 주문-상품 관계의 생성 및 수정 정보를 기록하는 메타데이터.

---
### Example SQL Query:

```sql
CREATE TABLE `store_schedule`
(
    store_schedule_id  BIGINT           NOT NULL AUTO_INCREMENT COMMENT '음식점 스케줄 ID',
    store_id           BIGINT           NOT NULL               COMMENT '음식점 ID',
    day_of_week        ENUM('MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT', 'SUN') 
                                        NOT NULL                COMMENT '요일',
    open_time          TIME             NOT NULL               COMMENT '오픈 시간',
    close_time         TIME             NOT NULL               COMMENT '종료 시간',
    delete_yn          VARCHAR(1)       NOT NULL DEFAULT 'N'   COMMENT '삭제 여부',
    created_user_id    BIGINT           NOT NULL               COMMENT '등록자 ID',
    created_datetime   DATETIME         NOT NULL               COMMENT '등록 일시',
    modified_user_id   BIGINT           NOT NULL               COMMENT '수정자 ID',
    modified_datetime  DATETIME         NOT NULL               COMMENT '수정 일시',

    PRIMARY KEY (store_schedule_id),
    CONSTRAINT fk_store_schedule_store FOREIGN KEY (store_id) REFERENCES `store`(store_id)
) ENGINE = INNODB DEFAULT CHARSET=utf8mb4 COMMENT='음식점 스케줄';
```

---

