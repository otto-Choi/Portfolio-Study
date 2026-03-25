# SQL_MASTER 4주차 정규과제

📌SQL MASTER 정규과제는 매주 정해진 분량의 『*데이터 분석을 위한 SQL 레시피*』 를 읽고 학습하는 것입니다. 이번 주는 아래의 **SQL_MASTER_4st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래 실습을 수행하며 학습 내용을 직접 적용해보세요. 단순히 결과를 재현하는 것이 아니라, SQL을 직접 작성하는 과정에서 개념을 스스로 정리하는 것이 중요합니다.

필요한 경우 교재와 추가 자료를 참고하여 이해를 보완하시기 바랍니다.

## SQL_MASTER_4st_TIL

### 5장 사용자를 파악하기 위한 데이터 추출
#### 1. 사용자 전체의 특징과 경향 찾기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.20~50    | ✅         |
| 2주차 | p.52~136   | ✅         |
| 3주차 | p.138~184  | ✅         |
| 4주차 | p.186~232 | ✅         |
| 5주차 | p.233~321 | 🍽️         |
| 6주차 | p.324~406 | 🍽️         |
| 7주차 | p.408~464 | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->


# 실습

## 0. 실습 규칙

1. 샘플 데이터 생성 코드는 **07_SQL_MASTER_Template/src** 경로에 장별로 정리되어 있습니다.
2. 아래 목차에 맞춰 해당 코드를 실행하여 샘플 데이터를 생성한 후, 각 장에서 요구하는 쿼리를 직접 작성해보시기 바랍니다.
3. 작성한 쿼리의 **실행 결과 화면도 함께 제출**해 주세요.
4. 단순히 교재의 예시 코드를 그대로 작성하는 것이 아니라, **제시된 로직을 충분히 이해한 뒤 교재를 보지 않고 스스로 쿼리를 구성**해보는 것을 권장합니다.
5. 교재 예시는 PostgreSQL, Hive, BigQuery 등 다양한 DBMS 기준으로 제시되어 있기 때문에, **MySQL이 아닌 다른 SQL 환경을 사용하여 실습을 진행해도 무방합니다.**
6. 다만, 사용 중인 DBMS에 맞는 문법으로 적절히 변환하여 작성하시기 바랍니다.

## 1. 사용자 전체의 특징과 경향 찾기

### 1-1 사용자의 액션 수 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->
stats는 단 하나의 행(전체 UU 수)만 가지고 있어서, 이걸 action_log의 모든 행에 붙여넣기 하려고 CROSS JOIN을 사용한다.<br>
쉽게 말하면 이 숫자를 모든 행에서 쓸 수 있게 복사해서 붙이는 용도이다.

```sql
WITH action_log_with_status AS (
    SELECT
        session,
        user_id,
        action,
        CASE WHEN COALESCE(user_id, '') <> '' THEN 'login' ELSE 'guest' END AS login_status,
        CASE
            WHEN COALESCE(MAX(user_id)
                OVER(PARTITION BY session ORDER BY stamp
                    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
                , '') <> ''
            THEN 'member'
            ELSE 'none'
        END AS member_status,
        stamp
    FROM action_log
)
SELECT
    COALESCE(action, 'all') AS action,
    COALESCE(login_status, 'all') AS login_status,
    COUNT(DISTINCT session) AS action_uu,
    COUNT(1) AS action_count
FROM action_log_with_status
GROUP BY ROLLUP(action, login_status);
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="664" height="648" alt="Image" src="https://github.com/user-attachments/assets/763e61f5-1bae-4264-b34a-2fd32d466442" />

<img width="557" height="391" alt="Image" src="https://github.com/user-attachments/assets/530aac92-4902-47fb-9700-20253be206da" />

### 1-2 연령별 구분 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH mst_users_with_int_birth_date AS (
    SELECT
        *,
        20170101 AS int_specific_date,
        CAST(replace(substring(birth_date, 1, 10), '-', '') AS integer) AS int_birth_date
    FROM mst_users
),
mst_users_with_age AS (
    SELECT
        *,
        floor((int_specific_date - int_birth_date) / 10000) AS age
    FROM mst_users_with_int_birth_date
),
mst_users_with_category AS (
    SELECT
        user_id,
        sex,
        age,
        CONCAT(
            CASE WHEN 20 <= age THEN sex ELSE '' END,
            CASE
                WHEN age BETWEEN 4  AND 12 THEN 'C'
                WHEN age BETWEEN 13 AND 19 THEN 'T'
                WHEN age BETWEEN 20 AND 34 THEN '1'
                WHEN age BETWEEN 35 AND 49 THEN '2'
                WHEN age >= 50             THEN '3'
            END
        ) AS category
    FROM mst_users_with_age
)
SELECT
    category,
    COUNT(1) AS user_count
FROM mst_users_with_category
GROUP BY category;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="288" height="239" alt="Image" src="https://github.com/user-attachments/assets/b709b4dc-5e19-44e8-b32f-667507d00417" />

### 1-3 연령별 구분의 특징 추출하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH mst_users_with_int_birth_date AS (
    SELECT
        *,
        20170101 AS int_specific_date,
        CAST(replace(substring(birth_date, 1, 10), '-', '') AS integer) AS int_birth_date
    FROM mst_users
),
mst_users_with_age AS (
    SELECT
        *,
        floor((int_specific_date - int_birth_date) / 10000) AS age
    FROM mst_users_with_int_birth_date
),
mst_users_with_category AS (
    SELECT
        user_id,
        sex,
        age,
        CONCAT(
            CASE WHEN 20 <= age THEN sex ELSE '' END,
            CASE
                WHEN age BETWEEN 4  AND 12 THEN 'C'
                WHEN age BETWEEN 13 AND 19 THEN 'T'
                WHEN age BETWEEN 20 AND 34 THEN '1'
                WHEN age BETWEEN 35 AND 49 THEN '2'
                WHEN age >= 50             THEN '3'
            END
        ) AS category
    FROM mst_users_with_age
)
SELECT
    p.category AS product_category,
    u.category AS user_category,
    COUNT(*) AS purchase_count
FROM action_log AS p
JOIN mst_users_with_category AS u
    ON p.user_id = u.user_id
WHERE action = 'purchase'
GROUP BY p.category, u.category
ORDER BY p.category, u.category;
```

<img width="539" height="142" alt="Image" src="https://github.com/user-attachments/assets/b592650e-8aed-4368-b348-92102a911783" />

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->
 
### 1-4 사용자의 방문 빈도 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH action_log_with_dt AS (
    SELECT
        *,
        substring(stamp, 1, 10) AS dt
    FROM action_log
),
action_day_count_per_user AS (
    SELECT
        user_id,
        COUNT(DISTINCT dt) AS action_day_count
    FROM action_log_with_dt
    WHERE dt BETWEEN '2016-11-01' AND '2016-11-07'
    GROUP BY user_id
)
SELECT
    action_day_count,
    COUNT(DISTINCT user_id) AS user_count,
    100.0
        * COUNT(DISTINCT user_id)
        / SUM(COUNT(DISTINCT user_id)) OVER()
        AS composition_ratio,
    100.0
        * SUM(COUNT(DISTINCT user_id))
            OVER(ORDER BY action_day_count ROWS UNBOUNDED PRECEDING)
        / SUM(COUNT(DISTINCT user_id)) OVER()
        AS cumulative_ratio
FROM action_day_count_per_user
GROUP BY action_day_count
ORDER BY action_day_count;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="694" height="72" alt="Image" src="https://github.com/user-attachments/assets/f4e37cb4-df81-4ae1-94a6-098c498cf774" />

### 1-5 벤 다이어그램으로 사용자 액션 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH user_action_flag AS (
    SELECT
        user_id,
        SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END)) AS has_purchase,
        SIGN(SUM(CASE WHEN action = 'review'   THEN 1 ELSE 0 END)) AS has_review,
        SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END)) AS has_favorite
    FROM action_log
    GROUP BY user_id
),
action_venn_diagram AS (
    SELECT
        has_purchase,
        has_review,
        has_favorite,
        COUNT(1) AS users
    FROM user_action_flag
    GROUP BY CUBE(has_purchase, has_review, has_favorite)
)
SELECT *
FROM action_venn_diagram
ORDER BY has_purchase, has_review, has_favorite;
```

```sql
WITH user_action_flag AS (
    SELECT
        user_id,
        SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END)) AS has_purchase,
        SIGN(SUM(CASE WHEN action = 'review'   THEN 1 ELSE 0 END)) AS has_review,
        SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END)) AS has_favorite
    FROM action_log
    GROUP BY user_id
),
action_venn_diagram AS (
    SELECT has_purchase, has_review, has_favorite, COUNT(1) AS users
    FROM user_action_flag GROUP BY has_purchase, has_review, has_favorite
    UNION ALL
    SELECT NULL, has_review, has_favorite, COUNT(1) AS users
    FROM user_action_flag GROUP BY has_review, has_favorite
    UNION ALL
    SELECT has_purchase, NULL, has_favorite, COUNT(1) AS users
    FROM user_action_flag GROUP BY has_purchase, has_favorite
    UNION ALL
    SELECT has_purchase, has_review, NULL, COUNT(1) AS users
    FROM user_action_flag GROUP BY has_purchase, has_review
    UNION ALL
    SELECT NULL, NULL, has_favorite, COUNT(1) AS users
    FROM user_action_flag GROUP BY has_favorite
    UNION ALL
    SELECT NULL, has_review, NULL, COUNT(1) AS users
    FROM user_action_flag GROUP BY has_review
    UNION ALL
    SELECT has_purchase, NULL, NULL, COUNT(1) AS users
    FROM user_action_flag GROUP BY has_purchase
    UNION ALL
    SELECT NULL, NULL, NULL, COUNT(1) AS users
    FROM user_action_flag
)
SELECT *
FROM action_venn_diagram
ORDER BY has_purchase, has_review, has_favorite;
```

```sql
WITH user_action_flag AS (
    SELECT
        user_id,
        SIGN(SUM(CASE WHEN action = 'purchase' THEN 1 ELSE 0 END)) AS has_purchase,
        SIGN(SUM(CASE WHEN action = 'review'   THEN 1 ELSE 0 END)) AS has_review,
        SIGN(SUM(CASE WHEN action = 'favorite' THEN 1 ELSE 0 END)) AS has_favorite
    FROM action_log
    GROUP BY user_id
),
action_venn_diagram AS (
    SELECT
        has_purchase,
        has_review,
        has_favorite,
        COUNT(1) AS users
    FROM user_action_flag
    GROUP BY CUBE(has_purchase, has_review, has_favorite)
)
SELECT
    CASE has_purchase
        WHEN 1 THEN 'purchase' WHEN 0 THEN 'not purchase' ELSE 'any'
    END AS has_purchase,
    CASE has_review
        WHEN 1 THEN 'review'   WHEN 0 THEN 'not review'   ELSE 'any'
    END AS has_review,
    CASE has_favorite
        WHEN 1 THEN 'favorite' WHEN 0 THEN 'not favorite' ELSE 'any'
    END AS has_favorite,
    users,
    100.0 * users
        / NULLIF(
            SUM(CASE WHEN has_purchase IS NULL
                     AND has_review   IS NULL
                     AND has_favorite IS NULL
                THEN users ELSE 0 END) OVER()
            , 0)
        AS ratio
FROM action_venn_diagram
ORDER BY has_purchase, has_review, has_favorite;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="725" height="493" alt="Image" src="https://github.com/user-attachments/assets/4bd85d64-aed9-4d94-82d9-d17046ab4a1f" />

모두 같은 결과를 얻을 수 있음.

### 1-6 Decile 분석을 사용해 사용자를 10단계 그룹으로 나누기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH user_purchase_amount AS (
    SELECT
        user_id,
        SUM(amount) AS purchase_amount
    FROM action_log
    WHERE action = 'purchase'
    GROUP BY user_id
),
users_with_decile AS (
    SELECT
        user_id,
        purchase_amount,
        ntile(10) OVER (ORDER BY purchase_amount DESC) AS decile
    FROM user_purchase_amount
),
decile_with_purchase_amount AS (
    SELECT
        decile,
        SUM(purchase_amount) AS amount,
        AVG(purchase_amount) AS avg_amount,
        SUM(SUM(purchase_amount)) OVER (ORDER BY decile) AS cumulative_amount,
        SUM(SUM(purchase_amount)) OVER () AS total_amount
    FROM users_with_decile
    GROUP BY decile
)
SELECT
    decile,
    amount,
    avg_amount,
    100.0 * amount / total_amount AS total_ratio,
    100.0 * cumulative_amount / total_amount AS cumulative_ratio
FROM decile_with_purchase_amount;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="748" height="110" alt="Image" src="https://github.com/user-attachments/assets/1a94828c-d83a-4ad5-a65a-3a3ad762edf5" />

### 1-7 RFM 분석으로 사용자를 3가지 관점의 그룹으로 나누기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

<img width="652" height="396" alt="Image" src="https://github.com/user-attachments/assets/19e1143e-0113-4bc8-a126-6f6c9ab17285" />

이러한 형태로 고객군을 분류하여 적절한 대응책을 수립하는 것이 흥미롭게 느껴졌음.

```sql
WITH purchase_log AS (
    SELECT
        user_id,
        amount,
        substring(stamp, 1, 10) AS dt
    FROM action_log
    WHERE action = 'purchase'
),
user_rfm AS (
    SELECT
        user_id,
        MAX(dt) AS recent_date,
        CURRENT_DATE - MAX(dt::date) AS recency,
        COUNT(dt) AS frequency,
        SUM(amount) AS monetary
    FROM purchase_log
    GROUP BY user_id
),
user_rfm_rank AS (
    SELECT
        user_id,
        recent_date,
        recency,
        frequency,
        monetary,
        CASE
            WHEN recency < 14 THEN 5
            WHEN recency < 28 THEN 4
            WHEN recency < 60 THEN 3
            WHEN recency < 90 THEN 2
            ELSE 1
        END AS r,
        CASE
            WHEN 20 <= frequency THEN 5
            WHEN 10 <= frequency THEN 4
            WHEN  5 <= frequency THEN 3
            WHEN  2 <= frequency THEN 2
            WHEN  1  = frequency THEN 1
        END AS f,
        CASE
            WHEN 300000 <= monetary THEN 5
            WHEN 100000 <= monetary THEN 4
            WHEN  30000 <= monetary THEN 3
            WHEN   5000 <= monetary THEN 2
            ELSE 1
        END AS m
    FROM user_rfm
),
mst_rfm_index AS (
         SELECT 1 AS rfm_index
    UNION ALL SELECT 2 AS rfm_index
    UNION ALL SELECT 3 AS rfm_index
    UNION ALL SELECT 4 AS rfm_index
    UNION ALL SELECT 5 AS rfm_index
),
rfm_flag AS (
    SELECT
        m.rfm_index,
        CASE WHEN m.rfm_index = r.r THEN 1 ELSE 0 END AS r_flag,
        CASE WHEN m.rfm_index = r.f THEN 1 ELSE 0 END AS f_flag,
        CASE WHEN m.rfm_index = r.m THEN 1 ELSE 0 END AS m_flag
    FROM mst_rfm_index AS m
    CROSS JOIN user_rfm_rank AS r
)
SELECT
    rfm_index,
    SUM(r_flag) AS r,
    SUM(f_flag) AS f,
    SUM(m_flag) AS m
FROM rfm_flag
GROUP BY rfm_index
ORDER BY rfm_index DESC;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="456" height="215" alt="Image" src="https://github.com/user-attachments/assets/e62cd030-e583-4132-bc6a-5ba21aed195e" />

### 🎉 수고하셨습니다.
