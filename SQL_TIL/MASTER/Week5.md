# SQL_MASTER 5주차 정규과제

📌SQL MASTER 정규과제는 매주 정해진 분량의 『*데이터 분석을 위한 SQL 레시피*』 를 읽고 학습하는 것입니다. 이번 주는 아래의 **SQL_MASTER_5st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래 실습을 수행하며 학습 내용을 직접 적용해보세요. 단순히 결과를 재현하는 것이 아니라, SQL을 직접 작성하는 과정에서 개념을 스스로 정리하는 것이 중요합니다.

필요한 경우 교재와 추가 자료를 참고하여 이해를 보완하시기 바랍니다.

## SQL_MASTER_5st_TIL

### 5장 사용자를 파악하기 위한 데이터 추출
#### 2. 시계열에 따른 사용자 전체의 상태 변화 찾기
#### 3. 시계열에 따른 사용자의 개별적인 행동 분석하기 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.20~50    | ✅         |
| 2주차 | p.52~136   | ✅         |
| 3주차 | p.138~184  | ✅         |
| 4주차 | p.186~232 | ✅         |
| 5주차 | p.233~321 | ✅         |
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

## 2. 시계열에 따른 사용자 전체의 상태 변화 찾기

### 2-1 등록 수의 추이와 경향 보기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH mst_users_with_year_month AS (
  SELECT
    *,
    substring(register_date, 1, 7) AS year_month
  FROM
    mst_users
)
SELECT
  year_month,
  COUNT(DISTINCT user_id) AS register_count,
  COUNT(DISTINCT CASE WHEN register_device = 'pc'  THEN user_id END) AS register_pc,
  COUNT(DISTINCT CASE WHEN register_device = 'sp'  THEN user_id END) AS register_sp,
  COUNT(DISTINCT CASE WHEN register_device = 'app' THEN user_id END) AS register_app
FROM
  mst_users_with_year_month
GROUP BY
  year_month;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="695" height="341" alt="Image" src="https://github.com/user-attachments/assets/1d79370a-e4f5-4626-92f4-d3734c2f973f" />

### 2-2 지속률과 정착률 산출하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

- 지속률: 등록일 기준으로 이후 지정일 동안 사용자가 서비스를 얼마나 이용했는지 나타내는 지표
- 정착률: 등록일 기준으로 이후 지정한 7일 동안 사용자가 서비스를 사용했는지 나타내는 지표

- CAST 함수를 통해 데이터 타입 변환 가능

```sql
WITH action_log_with_mst_users AS (
  SELECT
    u.user_id,
    u.register_date,
    CAST(a.stamp AS date) AS action_date,
    MAX(CAST(a.stamp AS date)) OVER() AS latest_date,
    CAST(u.register_date::date + '1 day'::interval AS date) AS next_day_1
  FROM
    mst_users u
    LEFT OUTER JOIN action_log a
    ON u.user_id = a.user_id
),
user_action_flag AS (
  SELECT
    user_id,
    register_date,
    SIGN(
      SUM(
        CASE WHEN next_day_1 <= latest_date THEN
          CASE WHEN next_day_1 = action_date THEN 1 ELSE 0 END
        END
      )
    ) AS next_1_day_action
  FROM
    action_log_with_mst_users
  GROUP BY
    user_id, register_date
)
SELECT
  register_date,
  AVG(100.0 * next_1_day_action) AS repeat_rate_1_day
FROM
  user_action_flag
GROUP BY
  register_date
ORDER BY
  register_date;
  ```


```SQL
WITH repeat_interval(index_name, interval_begin_date, interval_end_date) AS (
  VALUES
    ('07 day retention',  1,  7),
    ('14 day retention',  8, 14),
    ('21 day retention', 15, 21),
    ('28 day retention', 22, 28)
),
action_log_with_index_date AS (
  SELECT
    u.user_id,
    u.register_date,
    CAST(a.stamp AS date) AS action_date,
    MAX(CAST(a.stamp AS date)) OVER() AS latest_date,
    r.index_name,
    CAST(u.register_date::date + '1 day'::interval * r.interval_begin_date AS date) AS index_begin_date,
    CAST(u.register_date::date + '1 day'::interval * r.interval_end_date AS date) AS index_end_date
  FROM
    mst_users AS u
    LEFT OUTER JOIN action_log AS a
    ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
),
user_action_flag AS (
  SELECT
    user_id,
    register_date,
    index_name,
    SIGN(
      SUM(
        CASE WHEN index_end_date <= latest_date THEN
          CASE WHEN action_date BETWEEN index_begin_date AND index_end_date
            THEN 1 ELSE 0
          END
        END
      )
    ) AS index_date_action
  FROM
    action_log_with_index_date
  GROUP BY
    user_id, register_date, index_name, index_begin_date, index_end_date
)
SELECT
  register_date,
  index_name,
  AVG(100.0 * index_date_action) AS index_rate
FROM
  user_action_flag
GROUP BY
  register_date, index_name
ORDER BY
  register_date, index_name;
```

```sql
WITH repeat_interval(index_name, interval_begin_date, interval_end_date) AS (
  VALUES
    ('01 day repeat',  1,  1),
    ('02 day repeat',  2,  2),
    ('03 day repeat',  3,  3),
    ('04 day repeat',  4,  4),
    ('05 day repeat',  5,  5),
    ('06 day repeat',  6,  6),
    ('07 day repeat',  7,  7),
    ('07 day retention',  1,  7),
    ('14 day retention',  8, 14),
    ('21 day retention', 15, 21),
    ('28 day retention', 22, 28)
),
action_log_with_index_date AS (
  SELECT
    u.user_id,
    u.register_date,
    CAST(a.stamp AS date) AS action_date,
    MAX(CAST(a.stamp AS date)) OVER() AS latest_date,
    r.index_name,
    CAST(u.register_date::date + '1 day'::interval * r.interval_begin_date AS date) AS index_begin_date,
    CAST(u.register_date::date + '1 day'::interval * r.interval_end_date AS date) AS index_end_date
  FROM
    mst_users AS u
    LEFT OUTER JOIN action_log AS a
    ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
),
user_action_flag AS (
  SELECT
    user_id,
    register_date,
    index_name,
    SIGN(
      SUM(
        CASE WHEN index_end_date <= latest_date THEN
          CASE WHEN action_date BETWEEN index_begin_date AND index_end_date
            THEN 1 ELSE 0
          END
        END
      )
    ) AS index_date_action
  FROM
    action_log_with_index_date
  GROUP BY
    user_id, register_date, index_name, index_begin_date, index_end_date
)
SELECT
  index_name,
  AVG(100.0 * index_date_action) AS repeat_rate
FROM
  user_action_flag
GROUP BY
  index_name
ORDER BY
  index_name;
```
<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="446" height="514" alt="Image" src="https://github.com/user-attachments/assets/967f5693-44f5-4f19-9110-7ee93045d736" />

<img width="482" height="681" alt="Image" src="https://github.com/user-attachments/assets/22362ad6-05c8-464b-9e31-b21935af63b9" />

<img width="393" height="397" alt="Image" src="https://github.com/user-attachments/assets/392e0088-666e-44a9-bde7-dddfd0bac9a7" />

### 2-3 지속과 정착에 영향을 주는 액션 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH repeat_interval(index_name, interval_begin_date, interval_end_date) AS (
  VALUES ('01 day repeat', 1, 1)
),
action_log_with_index_date AS (
  SELECT
    u.user_id,
    u.register_date,
    CAST(a.stamp AS date) AS action_date,
    MAX(CAST(a.stamp AS date)) OVER() AS latest_date,
    r.index_name,
    CAST(u.register_date::date + '1 day'::interval * r.interval_begin_date AS date) AS index_begin_date,
    CAST(u.register_date::date + '1 day'::interval * r.interval_end_date AS date) AS index_end_date
  FROM
    mst_users AS u
    LEFT OUTER JOIN action_log AS a
    ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
),
user_action_flag AS (
  SELECT
    user_id,
    register_date,
    index_name,
    SIGN(
      SUM(
        CASE WHEN index_end_date <= latest_date THEN
          CASE WHEN action_date BETWEEN index_begin_date AND index_end_date
            THEN 1 ELSE 0
          END
        END
      )
    ) AS index_date_action
  FROM
    action_log_with_index_date
  GROUP BY
    user_id, register_date, index_name, index_begin_date, index_end_date
),
mst_actions(action) AS (
       SELECT 'view'
  UNION ALL SELECT 'comment'
  UNION ALL SELECT 'follow'
),
mst_user_actions AS (
  SELECT
    u.user_id,
    u.register_date,
    a.action
  FROM
    mst_users AS u
    CROSS JOIN mst_actions AS a
),
register_action_flag AS (
  SELECT DISTINCT
    m.user_id,
    m.register_date,
    m.action,
    CASE
      WHEN a.action IS NOT NULL THEN 1
      ELSE 0
    END AS do_action,
    f.index_name,
    f.index_date_action
  FROM
    mst_user_actions AS m
    LEFT JOIN action_log AS a
    ON m.user_id = a.user_id
    AND CAST(m.register_date AS date) = CAST(a.stamp AS date)
    AND m.action = a.action
    LEFT JOIN user_action_flag AS f
    ON m.user_id = f.user_id
  WHERE
    f.index_date_action IS NOT NULL
)
SELECT
  action,
  COUNT(1) AS users,
  AVG(100.0 * do_action) AS usage_rate,
  index_name,
  AVG(CASE do_action WHEN 1 THEN 100.0 * index_date_action END) AS idx_rate,
  AVG(CASE do_action WHEN 0 THEN 100.0 * index_date_action END) AS no_action_idx_rate
FROM
  register_action_flag
GROUP BY
  index_name, action
ORDER BY
  index_name, action;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="658" height="154" alt="Image" src="https://github.com/user-attachments/assets/d9b135a2-0e24-4402-a949-62fccb859130" />
 
### 2-4 액션 수에 따른 정착률 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH repeat_interval(index_name, interval_begin_date, interval_end_date) AS (
  VALUES ('14 day retention', 8, 14)
),
action_log_with_index_date AS (
  SELECT
    u.user_id,
    u.register_date,
    CAST(a.stamp AS date) AS action_date,
    MAX(CAST(a.stamp AS date)) OVER() AS latest_date,
    r.index_name,
    CAST(u.register_date::date + '1 day'::interval * r.interval_begin_date AS date) AS index_begin_date,
    CAST(u.register_date::date + '1 day'::interval * r.interval_end_date AS date) AS index_end_date
  FROM
    mst_users AS u
    LEFT OUTER JOIN action_log AS a
    ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
),
user_action_flag AS (
  SELECT
    user_id,
    register_date,
    index_name,
    SIGN(
      SUM(
        CASE WHEN index_end_date <= latest_date THEN
          CASE WHEN action_date BETWEEN index_begin_date AND index_end_date
            THEN 1 ELSE 0
          END
        END
      )
    ) AS index_date_action
  FROM
    action_log_with_index_date
  GROUP BY
    user_id, register_date, index_name, index_begin_date, index_end_date
),
mst_action_bucket(action, min_count, max_count) AS (
  VALUES
    ('comment',  0,  0),
    ('comment',  1,  5),
    ('comment',  6, 10),
    ('comment', 11, 9999),
    ('follow',   0,  0),
    ('follow',   1,  5),
    ('follow',   6, 10),
    ('follow',  11, 9999)
),
mst_user_action_bucket AS (
  SELECT
    u.user_id,
    u.register_date,
    a.action,
    a.min_count,
    a.max_count
  FROM
    mst_users AS u
    CROSS JOIN mst_action_bucket AS a
),
register_action_flag AS (
  SELECT
    m.user_id,
    m.action,
    m.min_count,
    m.max_count,
    COUNT(a.action) AS action_count,
    CASE
      WHEN COUNT(a.action) BETWEEN m.min_count AND m.max_count THEN 1
      ELSE 0
    END AS achieve,
    f.index_name,
    f.index_date_action
  FROM
    mst_user_action_bucket AS m
    LEFT JOIN action_log AS a
    ON m.user_id = a.user_id
    AND CAST(a.stamp AS date) BETWEEN CAST(m.register_date AS date)
      AND CAST(m.register_date AS date) + interval '7 days'
    AND m.action = a.action
    LEFT JOIN user_action_flag AS f
    ON m.user_id = f.user_id
  WHERE
    f.index_date_action IS NOT NULL
  GROUP BY
    m.user_id,
    m.action,
    m.min_count,
    m.max_count,
    f.index_name,
    f.index_date_action
)
SELECT
  action,
  min_count || ' ~ ' || max_count AS count_range,
  SUM(CASE achieve WHEN 1 THEN 1 ELSE 0 END) AS achieve,
  index_name,
  AVG(CASE achieve WHEN 1 THEN 100.0 * index_date_action END) AS achieve_index_rate
FROM
  register_action_flag
GROUP BY
  index_name, action, min_count, max_count
ORDER BY
  index_name, action, min_count;```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="648" height="301" alt="Image" src="https://github.com/user-attachments/assets/af133683-6f64-4e7c-b19e-f20df0c2f3f9" />

### 2-5 사용 일수에 따른 정착률 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH repeat_interval(index_name, interval_begin_date, interval_end_date) AS (
  VALUES ('28 day retention', 22, 28)
),
action_log_with_index_date AS (
  SELECT
    u.user_id,
    u.register_date,
    CAST(a.stamp AS date) AS action_date,
    MAX(CAST(a.stamp AS date)) OVER() AS latest_date,
    r.index_name,
    CAST(u.register_date::date + '1 day'::interval * r.interval_begin_date AS date) AS index_begin_date,
    CAST(u.register_date::date + '1 day'::interval * r.interval_end_date AS date) AS index_end_date
  FROM
    mst_users AS u
    LEFT OUTER JOIN action_log AS a
    ON u.user_id = a.user_id
    CROSS JOIN repeat_interval AS r
),
user_action_flag AS (
  SELECT
    user_id,
    register_date,
    index_name,
    SIGN(
      SUM(
        CASE WHEN index_end_date <= latest_date THEN
          CASE WHEN action_date BETWEEN index_begin_date AND index_end_date
            THEN 1 ELSE 0
          END
        END
      )
    ) AS index_date_action
  FROM
    action_log_with_index_date
  GROUP BY
    user_id, register_date, index_name, index_begin_date, index_end_date
),
register_action_flag AS (
  SELECT
    m.user_id,
    COUNT(DISTINCT CAST(a.stamp AS date)) AS dt_count,
    f.index_name,
    f.index_date_action
  FROM
    mst_users AS m
    LEFT JOIN action_log AS a
    ON m.user_id = a.user_id
    AND CAST(a.stamp AS date) BETWEEN CAST(m.register_date AS date) + interval '1 day'
      AND CAST(m.register_date AS date) + interval '8 days'
    LEFT JOIN user_action_flag AS f
    ON m.user_id = f.user_id
  WHERE
    f.index_date_action IS NOT NULL
  GROUP BY
    m.user_id,
    f.index_name,
    f.index_date_action
)
SELECT
  dt_count AS dates,
  COUNT(user_id) AS users,
  100.0 * COUNT(user_id) / SUM(COUNT(user_id)) OVER() AS user_ratio,
  100.0
    * SUM(COUNT(user_id))
        OVER(ORDER BY index_name, dt_count ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
    / SUM(COUNT(user_id)) OVER() AS cum_ratio,
  SUM(index_date_action) AS achieve_users,
  AVG(100.0 * index_date_action) AS achieve_ratio
FROM
  register_action_flag
GROUP BY
  index_name, dt_count
ORDER BY
  index_name, dt_count;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="531" height="234" alt="Image" src="https://github.com/user-attachments/assets/4db5b586-2c75-43ed-9ea1-2a4c72701dfc" />

데이터셋이 완전하지 않아 적절한 결과가 나오지 않는 것으로 보인다.

### 2-6 사용자의 잔존율 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH mst_intervals(interval_month) AS (
  VALUES (1), (2), (3), (4), (5), (6),
         (7), (8), (9), (10), (11), (12)
),
mst_users_with_index_month AS (
  SELECT
    u.user_id,
    u.register_date,
    CAST(u.register_date::date + i.interval_month * '1 month'::interval AS date) AS index_date,
    substring(u.register_date, 1, 7) AS register_month,
    substring(CAST(
      u.register_date::date + i.interval_month * '1 month'::interval
    AS text), 1, 7) AS index_month
  FROM
    mst_users AS u
    CROSS JOIN mst_intervals AS i
),
action_log_in_month AS (
  SELECT DISTINCT
    user_id,
    substring(stamp, 1, 7) AS action_month
  FROM
    action_log
)
SELECT
  u.register_month,
  u.index_month,
  SUM(CASE WHEN a.action_month IS NOT NULL THEN 1 ELSE 0 END) AS users,
  AVG(CASE WHEN a.action_month IS NOT NULL THEN 100.0 ELSE 0.0 END) AS retension_rate
FROM
  mst_users_with_index_month AS u
  LEFT JOIN action_log_in_month AS a
  ON u.user_id = a.user_id
  AND u.index_month = a.action_month
GROUP BY
  u.register_month, u.index_month
ORDER BY
  u.register_month, u.index_month;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="627" height="699" alt="Image" src="https://github.com/user-attachments/assets/f56c1481-cef2-4a76-8ecf-a77f9f7db2b2" />

위와 마찬가지. 집계되는 유저 수가 2명만 존재한다.

### 2-7 방문 빈도를 기반으로 사용자 속성을 정의하고 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

- MAU(Monthly Acive Users): 월 사용자 수
    - 신규 사용자: 이번 달에 등록한 신규 사용자
    - 리피트 사용자: 이전 달에도 사용했던 사용자
    - 컴백 사용자: 복귀 사용자

```sql
WITH monthly_user_action AS (
  SELECT DISTINCT
    u.user_id,
    substring(u.register_date, 1, 7) AS register_month,
    substring(l.stamp, 1, 7) AS action_month,
    substring(CAST(l.stamp::date - interval '1 month' AS text), 1, 7) AS action_month_priv
  FROM
    mst_users AS u
    JOIN action_log AS l
    ON u.user_id = l.user_id
),
monthly_user_with_type AS (
  SELECT
    action_month,
    user_id,
    CASE
      WHEN register_month = action_month THEN 'new_user'
      WHEN action_month_priv
        = LAG(action_month) OVER(PARTITION BY user_id ORDER BY action_month)
        THEN 'repeat_user'
      ELSE 'come_back_user'
    END AS c,
    action_month_priv
  FROM
    monthly_user_action
),
monthly_users AS (
  SELECT
    m1.action_month,
    COUNT(m1.user_id) AS mau,
    COUNT(CASE WHEN m1.c = 'new_user'       THEN 1 END) AS new_users,
    COUNT(CASE WHEN m1.c = 'repeat_user'    THEN 1 END) AS repeat_users,
    COUNT(CASE WHEN m1.c = 'come_back_user' THEN 1 END) AS come_back_users,
    COUNT(CASE WHEN m1.c = 'repeat_user' AND m0.c = 'new_user'       THEN 1 END) AS new_repeat_users,
    COUNT(CASE WHEN m1.c = 'repeat_user' AND m0.c = 'repeat_user'    THEN 1 END) AS continuous_repeat_users,
    COUNT(CASE WHEN m1.c = 'repeat_user' AND m0.c = 'come_back_user' THEN 1 END) AS come_back_repeat_users
  FROM
    monthly_user_with_type AS m1
    LEFT OUTER JOIN monthly_user_with_type AS m0
    ON m1.user_id = m0.user_id
    AND m1.action_month_priv = m0.action_month
  GROUP BY
    m1.action_month
)
SELECT
  action_month,
  mau,
  new_users,
  repeat_users,
  come_back_users,
  new_repeat_users,
  continuous_repeat_users,
  come_back_repeat_users,
  100.0 * new_repeat_users
    / NULLIF(LAG(new_users) OVER(ORDER BY action_month), 0)
    AS priv_new_repeat_ratio,
  100.0 * continuous_repeat_users
    / NULLIF(LAG(repeat_users) OVER(ORDER BY action_month), 0)
    AS priv_continuous_repeat_ratio,
  100.0 * come_back_repeat_users
    / NULLIF(LAG(come_back_users) OVER(ORDER BY action_month), 0)
    AS priv_come_back_repeat_ratio
FROM
  monthly_users
ORDER BY
  action_month;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="357" height="310" alt="Image" src="https://github.com/user-attachments/assets/3f9926a8-dad0-4ed6-be83-3422973813a5" />

### 2-8 방문 종류를 기반으로 성장지수 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH unique_action_log AS (
  SELECT DISTINCT
    user_id,
    substring(stamp, 1, 10) AS action_date
  FROM
    action_log
),
mst_calendar AS (
       SELECT '2016-10-01' AS dt
  UNION ALL SELECT '2016-10-02' AS dt
  UNION ALL SELECT '2016-10-03' AS dt
  UNION ALL SELECT '2016-11-04' AS dt
),
target_date_with_user AS (
  SELECT
    c.dt AS target_date,
    u.user_id,
    u.register_date,
    u.withdraw_date
  FROM
    mst_users AS u
    CROSS JOIN mst_calendar AS c
),
user_status_log AS (
  SELECT
    u.target_date,
    u.user_id,
    u.register_date,
    u.withdraw_date,
    a.action_date,
    CASE WHEN u.register_date = a.action_date THEN 1 ELSE 0 END AS is_new,
    CASE WHEN u.withdraw_date = a.action_date THEN 1 ELSE 0 END AS is_exit,
    CASE WHEN u.target_date = a.action_date THEN 1 ELSE 0 END AS is_access,
    LAG(CASE WHEN u.target_date = a.action_date THEN 1 ELSE 0 END)
      OVER(PARTITION BY u.user_id ORDER BY u.target_date) AS was_access
  FROM
    target_date_with_user AS u
    LEFT JOIN unique_action_log AS a
    ON u.user_id = a.user_id
    AND u.target_date = a.action_date
  WHERE
    u.register_date <= u.target_date
    AND (
      u.withdraw_date IS NULL
      OR u.target_date <= u.withdraw_date
    )
),
user_growth_index AS (
  SELECT
    *,
    CASE
      WHEN is_new + is_exit = 1 THEN
        CASE
          WHEN is_new = 1 THEN 'signup'
          WHEN is_exit = 1 THEN 'exit'
        END
      WHEN is_new + is_exit = 0 THEN
        CASE
          WHEN was_access = 0 AND is_access = 1 THEN 'reactivation'
          WHEN was_access = 1 AND is_access = 0 THEN 'deactivation'
        END
    END AS growth_index
  FROM
    user_status_log
)
SELECT
  target_date,
  SUM(CASE growth_index WHEN 'signup'        THEN  1 ELSE 0 END) AS signup,
  SUM(CASE growth_index WHEN 'reactivation'  THEN  1 ELSE 0 END) AS reactivation,
  SUM(CASE growth_index WHEN 'deactivation'  THEN -1 ELSE 0 END) AS deactivation,
  SUM(CASE growth_index WHEN 'exit'          THEN -1 ELSE 0 END) AS exit,
  SUM(
    CASE growth_index
      WHEN 'signup'       THEN  1
      WHEN 'reactivation' THEN  1
      WHEN 'deactivation' THEN -1
      WHEN 'exit'         THEN -1
      ELSE 0
    END
  ) AS growth_index
FROM
  user_growth_index
GROUP BY
  target_date
ORDER BY
  target_date;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="592" height="315" alt="Image" src="https://github.com/user-attachments/assets/b04dfa4a-d7de-49ef-bdfd-88f4f5042da1" />

### 2-9 지표 개선 방법 익히기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

<!-- ```sql
여기에 코드를 적어주세요.
``` -->

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->


## 3. 시계열에 따른 사용자의 개별적인 행동 분석하기 

### 3-1 사용자의 액션 간격 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH reservations(reservation_id, register_date, visit_date, days) AS (
  VALUES
    (1, date '2016-09-01', date '2016-10-01', 3),
    (2, date '2016-09-20', date '2016-10-01', 2),
    (3, date '2016-09-30', date '2016-11-20', 2),
    (4, date '2016-10-01', date '2017-01-03', 2),
    (5, date '2016-11-01', date '2016-12-28', 3)
)
SELECT
  reservation_id,
  register_date,
  visit_date,
  visit_date::date - register_date::date AS lead_time
FROM
  reservations;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="602" height="201" alt="Image" src="https://github.com/user-attachments/assets/ddf66a67-3d7e-425d-985e-a7fc5608ffe6" />

### 3-2 카트 추가 후에 구매했는지 파악하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH row_action_log AS (
  SELECT
    dt,
    user_id,
    action,
    regexp_split_to_table(products, ',') AS product_id,
    stamp
  FROM
    action_log
),
action_time_stats AS (
  SELECT
    user_id,
    product_id,
    MIN(CASE action WHEN 'add_cart' THEN dt END) AS dt,
    MIN(CASE action WHEN 'add_cart' THEN stamp END) AS add_cart_time,
    MIN(CASE action WHEN 'purchase' THEN stamp END) AS purchase_time,
    EXTRACT(epoch FROM
      MIN(CASE action WHEN 'purchase' THEN stamp::timestamp END)
      - MIN(CASE action WHEN 'add_cart' THEN stamp::timestamp END)
    ) AS lead_time
  FROM
    row_action_log
  GROUP BY
    user_id, product_id
),
purchase_lead_time_flag AS (
  SELECT
    user_id,
    product_id,
    dt,
    CASE WHEN lead_time <=  1 * 60 * 60 THEN 1 ELSE 0 END AS purchase_1_hour,
    CASE WHEN lead_time <=  6 * 60 * 60 THEN 1 ELSE 0 END AS purchase_6_hours,
    CASE WHEN lead_time <= 24 * 60 * 60 THEN 1 ELSE 0 END AS purchase_24_hours,
    CASE WHEN lead_time <= 48 * 60 * 60 THEN 1 ELSE 0 END AS purchase_48_hours,
    CASE
      WHEN lead_time IS NULL OR NOT (lead_time <= 48 * 60 * 60) THEN 1
      ELSE 0
    END AS not_purchase
  FROM
    action_time_stats
)
SELECT
  dt,
  COUNT(*) AS add_cart,
  SUM(purchase_1_hour)    AS purchase_1_hour,
  AVG(purchase_1_hour)    AS purchase_1_hour_rate,
  SUM(purchase_6_hours)   AS purchase_6_hours,
  AVG(purchase_6_hours)   AS purchase_6_hours_rate,
  SUM(purchase_24_hours)  AS purchase_24_hours,
  AVG(purchase_24_hours)  AS purchase_24_hours_rate,
  SUM(purchase_48_hours)  AS purchase_48_hours,
  AVG(purchase_48_hours)  AS purchase_48_hours_rate,
  SUM(not_purchase)       AS not_purchase,
  AVG(not_purchase)       AS not_purchase_rate
FROM
  purchase_lead_time_flag
GROUP BY
  dt;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="554" height="637" alt="Image" src="https://github.com/user-attachments/assets/052f4441-6b12-4737-af8d-f3a5d99a31e5" />

### 3-3 등록으로부터의 매출을 날짜별로 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH index_intervals(index_name, interval_begin_date, interval_end_date) AS (
  VALUES
    ('30 day sales amount', 0, 30),
    ('45 day sales amount', 0, 45),
    ('60 day sales amount', 0, 60)
),
mst_users_with_base_date AS (
  SELECT
    user_id,
    register_date AS base_date
  FROM
    mst_users
),
purchase_log_with_index_date AS (
  SELECT
    u.user_id,
    u.base_date,
    CAST(p.stamp AS date) AS action_date,
    MAX(CAST(p.stamp AS date)) OVER() AS latest_date,
    substring(u.base_date, 1, 7) AS month,
    i.index_name,
    CAST(u.base_date::date + '1 day'::interval * i.interval_begin_date AS date) AS index_begin_date,
    CAST(u.base_date::date + '1 day'::interval * i.interval_end_date AS date) AS index_end_date,
    p.amount
  FROM
    mst_users_with_base_date AS u
    LEFT OUTER JOIN action_log AS p
    ON u.user_id = p.user_id
    AND p.action = 'purchase'
    CROSS JOIN index_intervals AS i
),
user_purchase_amount AS (
  SELECT
    user_id,
    month,
    index_name,
    SUM(
      CASE WHEN index_end_date <= latest_date THEN
        CASE
          WHEN action_date BETWEEN index_begin_date AND index_end_date THEN amount
          ELSE 0
        END
      ELSE 0
      END
    ) AS index_date_amount
  FROM
    purchase_log_with_index_date
  GROUP BY
    user_id, month, index_name, index_begin_date, index_end_date
)
SELECT
  month,
  COUNT(user_id) AS users,
  index_name,
  COUNT(CASE WHEN index_date_amount > 0 THEN user_id END) AS purchase_uu,
  SUM(index_date_amount) AS total_amount,
  AVG(index_date_amount) AS avg_amount
FROM
  user_purchase_amount
GROUP BY
  month, index_name
ORDER BY
  month, index_name;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="601" height="576" alt="Image" src="https://github.com/user-attachments/assets/58bd0d43-df65-42d5-a84d-53655f31f48d" />

### 🎉 수고하셨습니다.
