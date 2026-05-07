# SQL_MASTER 6주차 정규과제

📌SQL MASTER 정규과제는 매주 정해진 분량의 『*데이터 분석을 위한 SQL 레시피*』 를 읽고 학습하는 것입니다. 이번 주는 아래의 **SQL_MASTER_6st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래 실습을 수행하며 학습 내용을 직접 적용해보세요. 단순히 결과를 재현하는 것이 아니라, SQL을 직접 작성하는 과정에서 개념을 스스로 정리하는 것이 중요합니다.

필요한 경우 교재와 추가 자료를 참고하여 이해를 보완하시기 바랍니다.

## SQL_MASTER_6st_TIL

### 6장 웹사이트에서의 행동을 파악하는 데이터 추출하기
#### 1. 사이트 전체의 특징/경향 찾기
#### 2. 사이트 내의 사용자 행동 파악하기
#### 3. 입력 양식 최적화하기 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.20~50    | ✅         |
| 2주차 | p.52~136   | ✅         |
| 3주차 | p.138~184  | ✅         |
| 4주차 | p.186~232 | ✅         |
| 5주차 | p.233~321 | ✅         |
| 6주차 | p.324~406 | ✅         |
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

## 1. 사이트 전체의 특징/경향 찾기

### 1-1 날짜별 방문자 수 / 방문 횟수 / 페이지 뷰 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
SELECT
  substring(stamp, 1, 10) AS dt,
  COUNT(DISTINCT long_session) AS access_users,
  COUNT(DISTINCT short_session) AS access_count,
  COUNT(*) AS page_view,
  1.0 * COUNT(*) / NULLIF(COUNT(DISTINCT long_session), 0) AS pv_per_user
FROM
  access_log
GROUP BY
  dt
ORDER BY
  dt;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="808" height="236" alt="Image" src="https://github.com/user-attachments/assets/4842c698-8e6b-4640-b092-d540ef5ea0d3" />

### 1-2 페이지별 쿠키 / 방문 횟수 / 페이지 뷰 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH access_log_with_path AS (
  SELECT
    *,
    substring(url from '//[^/]+([^?#]+)') AS url_path
  FROM
    access_log
),
access_log_with_split_path AS (
  SELECT
    *,
    split_part(url_path, '/', 2) AS path1,
    split_part(url_path, '/', 3) AS path2
  FROM
    access_log_with_path
),
access_log_with_page_name AS (
  SELECT
    *,
    CASE
      WHEN path1 = 'list' THEN
        CASE
          WHEN path2 = 'newly' THEN 'newly_list'
          ELSE 'category_list'
        END
      ELSE url_path
    END AS page_name
  FROM
    access_log_with_split_path
)
SELECT
  page_name,
  COUNT(DISTINCT short_session) AS access_count,
  COUNT(DISTINCT long_session) AS access_users,
  COUNT(*) AS page_view
FROM
  access_log_with_page_name
GROUP BY
  page_name
ORDER BY
  page_name;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="619" height="194" alt="Image" src="https://github.com/user-attachments/assets/f490f22a-4a2a-4c54-ac35-413f15528099" />

### 1-3 유입원별로 방문 횟수 또는 CVR 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH access_log_with_parse_info AS (
  SELECT
    *,
    substring(url from 'https?://([^/]*)') AS url_domain,
    substring(url from 'utm_source=([^&]*)') AS url_utm_source,
    substring(url from 'utm_medium=([^&]*)') AS url_utm_medium,
    substring(referrer from 'https?://([^/]*)') AS referrer_domain
  FROM
    access_log
),
access_log_with_via_info AS (
  SELECT
    *,
    ROW_NUMBER() OVER(ORDER BY stamp) AS log_id,
    CASE
      WHEN url_utm_source <> '' AND url_utm_medium <> '' THEN
        url_utm_source || '-' || url_utm_medium
      WHEN referrer_domain IN ('search.yahoo.co.jp', 'www.google.co.jp') THEN 'search'
      WHEN referrer_domain IN ('twitter.com', 'www.facebook.com') THEN 'social'
      ELSE 'other'
    END AS via
  FROM
    access_log_with_parse_info
  WHERE
    COALESCE(referrer_domain, '') NOT IN ('', url_domain)
),
access_log_with_purchase_amount AS (
  SELECT
    a.log_id,
    a.via,
    SUM(
      CASE
        WHEN p.stamp::date BETWEEN a.stamp::date AND a.stamp::date + '1 day'::interval
        THEN amount
      END
    ) AS amount
  FROM
    access_log_with_via_info AS a
  LEFT OUTER JOIN
    purchase_log AS p
    ON a.long_session = p.long_session
  GROUP BY
    a.log_id, a.via
)
SELECT
  via,
  COUNT(1) AS via_count,
  COUNT(amount) AS conversions,
  AVG(100.0 * SIGN(COALESCE(amount, 0))) AS cvr,
  SUM(COALESCE(amount, 0)) AS amount,
  AVG(1.0 * COALESCE(amount, 0)) AS avg_amount
FROM
  access_log_with_purchase_amount
GROUP BY
  via
ORDER BY
  cvr DESC;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->
 
<img width="781" height="226" alt="Image" src="https://github.com/user-attachments/assets/b249bcf2-437b-40f5-974e-fe4485eaba2e" />

### 1-4 접근 요일,시간대 파악하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH access_log_with_dow AS (
  SELECT
    stamp,
    date_part('dow', stamp::timestamp) AS dow,
    CAST(substring(stamp, 12, 2) AS int) * 60 * 60 +
    CAST(substring(stamp, 15, 2) AS int) * 60 +
    CAST(substring(stamp, 18, 2) AS int) AS whole_seconds,
    30 * 60 AS interval_seconds
  FROM access_log
),
access_log_with_floor_seconds AS (
  SELECT
    stamp,
    dow,
    CAST((floor(whole_seconds / interval_seconds) * interval_seconds) AS int) AS floor_seconds
  FROM access_log_with_dow
),
access_log_with_index AS (
  SELECT
    stamp,
    dow,
    lpad(floor(floor_seconds / (60 * 60))::text, 2, '0') || ':' ||
    lpad(floor(floor_seconds % (60 * 60) / 60)::text, 2, '0') || ':' ||
    lpad(floor(floor_seconds % 60)::text, 2, '0') AS index_time
  FROM access_log_with_floor_seconds
)
SELECT
  index_time,
  COUNT(CASE dow WHEN 0 THEN 1 END) AS sun,
  COUNT(CASE dow WHEN 1 THEN 1 END) AS mon,
  COUNT(CASE dow WHEN 2 THEN 1 END) AS tue,
  COUNT(CASE dow WHEN 3 THEN 1 END) AS wed,
  COUNT(CASE dow WHEN 4 THEN 1 END) AS thu,
  COUNT(CASE dow WHEN 5 THEN 1 END) AS fri,
  COUNT(CASE dow WHEN 6 THEN 1 END) AS sat
FROM
  access_log_with_index
GROUP BY
  index_time
ORDER BY
  index_time;
  ```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="774" height="442" alt="Image" src="https://github.com/user-attachments/assets/b1b7194a-b1bc-40f7-9368-603d89a2453f" />

## 2. 사이트 내의 사용자 행동 파악하기 

### 2-1 입구 페이지와 출구 페이지 파악하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH activity_log_with_landing_exit AS (
  SELECT
    session,
    path,
    stamp,
    FIRST_VALUE(path) OVER(
      PARTITION BY session
      ORDER BY stamp ASC
      ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS landing,
    LAST_VALUE(path) OVER(
      PARTITION BY session
      ORDER BY stamp ASC
      ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS exit
  FROM activity_log
),
landing_count AS (
  SELECT
    landing AS path,
    COUNT(DISTINCT session) AS count
  FROM
    activity_log_with_landing_exit
  GROUP BY landing
),
exit_count AS (
  SELECT
    exit AS path,
    COUNT(DISTINCT session) AS count
  FROM
    activity_log_with_landing_exit
  GROUP BY exit
)
SELECT 'landing' AS type, * FROM landing_count
UNION ALL
SELECT 'exit' AS type, * FROM exit_count;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="391" height="275" alt="Image" src="https://github.com/user-attachments/assets/334c8ecb-0015-4d17-937a-08e8ca76b4a4" />

### 2-2 이탈률과 직귀율 계산하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH activity_log_with_exit_flag AS (
  SELECT
    *,
    CASE
      WHEN ROW_NUMBER() OVER(PARTITION BY session ORDER BY stamp DESC) = 1 THEN 1
      ELSE 0
    END AS is_exit
  FROM
    activity_log
)
SELECT
  path,
  SUM(is_exit) AS exit_count,
  COUNT(1) AS page_view,
  AVG(100.0 * is_exit) AS exit_ratio
FROM
  activity_log_with_exit_flag
GROUP BY
  path;
```

```sql
WITH activity_log_with_landing_bounce_flag AS (
  SELECT
    *,
    CASE
      WHEN ROW_NUMBER() OVER(PARTITION BY session ORDER BY stamp ASC) = 1 THEN 1
      ELSE 0
    END AS is_landing,
    CASE
      WHEN COUNT(1) OVER(PARTITION BY session) = 1 THEN 1
      ELSE 0
    END AS is_bounce
  FROM
    activity_log
)
SELECT
  path,
  SUM(is_bounce) AS bounce_count,
  SUM(is_landing) AS landing_count,
  AVG(100.0 * CASE WHEN is_landing = 1 THEN is_bounce END) AS bounce_ratio
FROM
  activity_log_with_landing_bounce_flag
GROUP BY
  path;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="578" height="171" alt="Image" src="https://github.com/user-attachments/assets/8bfbae8e-d621-47da-8ff7-1affba356276" />

<img width="672" height="184" alt="Image" src="https://github.com/user-attachments/assets/aee17a71-7ca5-4f96-b892-cd0e7e4ec7c2" />

### 2-3 성과로 이어지는 페이지 파악하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH activity_log_with_conversion_flag AS (
  SELECT
    session,
    stamp,
    path,
    SIGN(SUM(CASE WHEN path = '/complete' THEN 1 ELSE 0 END) OVER(PARTITION BY session ORDER BY stamp DESC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)) AS has_conversion
  FROM
    activity_log
)
SELECT
  path,
  COUNT(DISTINCT session) AS sessions,
  SUM(has_conversion) AS conversions,
  1.0 * SUM(has_conversion) / COUNT(DISTINCT session) AS cvr
FROM
  activity_log_with_conversion_flag
GROUP BY
  path;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="584" height="283" alt="Image" src="https://github.com/user-attachments/assets/f3e6f4c4-399a-4783-b9f6-b94e6d145eb6" />

### 2-4 페이지 가치 산출하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH activity_log_with_conversion_flag AS (
  SELECT
    session,
    stamp,
    path,
    SIGN(SUM(CASE WHEN path = '/complete' THEN 1 ELSE 0 END) OVER(PARTITION BY session ORDER BY stamp DESC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)) AS has_conversion
  FROM
    activity_log
),
activity_log_with_conversion_assign AS (
  SELECT
    session,
    path,
    1000.0 / COUNT(1) OVER(PARTITION BY session) AS fair_assign,
    CASE
      WHEN ROW_NUMBER() OVER(PARTITION BY session ORDER BY stamp ASC) = 1 THEN 1000.0
      ELSE 0.0
    END AS first_assign,
    CASE
      WHEN ROW_NUMBER() OVER(PARTITION BY session ORDER BY stamp DESC) = 1 THEN 1000.0
      ELSE 0.0
    END AS last_assign,
    1000.0 * ROW_NUMBER() OVER(PARTITION BY session ORDER BY stamp ASC) /
    (COUNT(1) OVER(PARTITION BY session) * (COUNT(1) OVER(PARTITION BY session) + 1) / 2) AS decrease_assign,
    1000.0 * ROW_NUMBER() OVER(PARTITION BY session ORDER BY stamp DESC) /
    (COUNT(1) OVER(PARTITION BY session) * (COUNT(1) OVER(PARTITION BY session) + 1) / 2) AS increase_assign
  FROM
    activity_log_with_conversion_flag
  WHERE
    has_conversion = 1
    AND path NOT IN ('/input', '/confirm', '/complete')
),
page_total_values AS (
  SELECT
    path,
    SUM(fair_assign) AS sum_fair,
    SUM(first_assign) AS sum_first,
    SUM(last_assign) AS sum_last,
    SUM(decrease_assign) AS sum_dec,
    SUM(increase_assign) AS sum_inc
  FROM
    activity_log_with_conversion_assign
  GROUP BY
    path
),
page_total_cnt AS (
  SELECT
    path,
    COUNT(1) AS access_cnt
  FROM
    activity_log
  GROUP BY
    path
)
SELECT
  s.path,
  s.access_cnt,
  v.sum_fair / s.access_cnt AS avg_fair,
  v.sum_first / s.access_cnt AS avg_first,
  v.sum_last / s.access_cnt AS avg_last,
  v.sum_dec / s.access_cnt AS avg_dec,
  v.sum_inc / s.access_cnt AS avg_inc
FROM
  page_total_cnt AS s
JOIN
  page_total_values AS v
  ON s.path = v.path
ORDER BY
  s.access_cnt DESC;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="426" height="423" alt="Image" src="https://github.com/user-attachments/assets/f57e6f6f-fbcf-437e-ab40-52c1d22070d8" />

### 2-5 검색 조건들의 사용자 행동 가시화하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH activity_log_with_session_click_conversion_flag AS (
  SELECT
    session,
    stamp,
    path,
    search_type,
    CASE
      WHEN LAG(path) OVER(PARTITION BY session ORDER BY stamp DESC) = '/detail' THEN 1
      ELSE 0
    END AS has_session_click,
    SIGN(SUM(CASE WHEN path = '/complete' THEN 1 ELSE 0 END) OVER(PARTITION BY session ORDER BY stamp DESC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)) AS has_session_conversion
  FROM
    activity_log
)
SELECT
  session,
  stamp,
  path,
  search_type,
  has_session_click AS click,
  has_session_conversion AS cnv
FROM
  activity_log_with_session_click_conversion_flag
ORDER BY
  session,
  stamp;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="689" height="689" alt="Image" src="https://github.com/user-attachments/assets/fa14a149-9c0b-4cd2-b1b0-385e3093e947" />

### 2-6 폴아웃 리포트를 사용해 사용자 회유를 가시화하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH mst_fallout_step AS (
  SELECT 1 AS step, '/' AS path
  UNION ALL SELECT 2 AS step, '/search_list' AS path
  UNION ALL SELECT 3 AS step, '/detail' AS path
  UNION ALL SELECT 4 AS step, '/input' AS path
  UNION ALL SELECT 5 AS step, '/complete' AS path
),
activity_log_with_fallout_step AS (
  SELECT
    l.session,
    m.step,
    m.path,
    MAX(l.stamp) AS max_stamp,
    MIN(l.stamp) AS min_stamp
  FROM
    mst_fallout_step AS m
  JOIN
    activity_log AS l
    ON m.path = l.path
  GROUP BY
    l.session, m.step, m.path
),
activity_log_with_mod_fallout_step AS (
  SELECT
    session,
    step,
    path,
    max_stamp,
    LAG(min_stamp) OVER(PARTITION BY session ORDER BY step) AS lag_min_stamp,
    MIN(step) OVER(PARTITION BY session) AS min_step,
    COUNT(1) OVER(PARTITION BY session ORDER BY step ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS cum_count
  FROM
    activity_log_with_fallout_step
),
fallout_log AS (
  SELECT
    session,
    step,
    path
  FROM
    activity_log_with_mod_fallout_step
  WHERE
    min_step = 1 AND
    step = cum_count AND
    (lag_min_stamp IS NULL OR max_stamp >= lag_min_stamp)
)
SELECT
  step,
  path,
  COUNT(1) AS count,
  100.0 * COUNT(1) / FIRST_VALUE(COUNT(1)) OVER(ORDER BY step ASC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS first_trans_rate,
  100.0 * COUNT(1) / LAG(COUNT(1)) OVER(ORDER BY step ASC) AS step_trans_rate
FROM
  fallout_log
GROUP BY
  step, path
ORDER BY
  step;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="714" height="151" alt="Image" src="https://github.com/user-attachments/assets/7ce72bfd-718e-469a-9ae1-49ea45293b40" />

### 2-7 사이트 내부에서 사용자 흐름 파악하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH activity_log_with_lead_path AS (
  SELECT
    session,
    stamp,
    path AS path0,
    LEAD(path, 1) OVER(PARTITION BY session ORDER BY stamp ASC) AS path1,
    LEAD(path, 2) OVER(PARTITION BY session ORDER BY stamp ASC) AS path2
  FROM
    activity_log
),
raw_user_flow AS (
  SELECT
    path0,
    SUM(COUNT(1)) OVER() AS count0,
    COALESCE(path1, 'NULL') AS path1,
    SUM(COUNT(1)) OVER(PARTITION BY path0, path1) AS count1,
    COALESCE(path2, 'NULL') AS path2,
    COUNT(1) AS count2
  FROM
    activity_log_with_lead_path
  WHERE
    path0 = '/detail'
  GROUP BY
    path0, path1, path2
)
SELECT
  CASE
    WHEN COALESCE(LAG(path0) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> path0 THEN path0
  END AS path0,
  CASE
    WHEN COALESCE(LAG(path0) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> path0 THEN count0
  END AS count0,
  CASE
    WHEN COALESCE(LAG(path0 || path1) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> (path0 || path1) THEN path1
  END AS page1,
  CASE
    WHEN COALESCE(LAG(path0 || path1) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> (path0 || path1) THEN count1
  END AS count1,
  CASE
    WHEN COALESCE(LAG(path0 || path1) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> (path0 || path1) THEN 100.0 * count1 / count0
  END AS rate1,
  CASE
    WHEN COALESCE(LAG(path0 || path1 || path2) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> (path0 || path1 || path2) THEN path2
  END AS page2,
  CASE
    WHEN COALESCE(LAG(path0 || path1 || path2) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> (path0 || path1 || path2) THEN count2
  END AS count2,
  CASE
    WHEN COALESCE(LAG(path0 || path1 || path2) OVER(ORDER BY count1 DESC, count2 DESC), 'NOT FOUND') <> (path0 || path1 || path2) THEN 100.0 * count2 / count1
  END AS rate2
FROM
  raw_user_flow
ORDER BY
  count1 DESC, count2 DESC;
```

```sql
WITH activity_log_with_lag_path AS (
  SELECT
    session,
    stamp,
    path AS path0,
    COALESCE(LAG(path, 1) OVER(PARTITION BY session ORDER BY stamp ASC), 'NULL') AS path1,
    COALESCE(LAG(path, 2) OVER(PARTITION BY session ORDER BY stamp ASC), 'NULL') AS path2
  FROM
    activity_log
),
raw_user_flow AS (
  SELECT
    path0,
    SUM(COUNT(1)) OVER() AS count0,
    path1,
    SUM(COUNT(1)) OVER(PARTITION BY path0, path1) AS count1,
    path2,
    COUNT(1) AS count2
  FROM
    activity_log_with_lag_path
  WHERE
    path0 = '/detail'
  GROUP BY
    path0, path1, path2
)
SELECT
  path2,
  count2,
  100.0 * count2 / count1 AS rate2,
  path1,
  count1,
  100.0 * count1 / count0 AS rate1,
  path0,
  count0
FROM
  raw_user_flow
ORDER BY
  count1 DESC, count2 DESC;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="758" height="180" alt="Image" src="https://github.com/user-attachments/assets/20df74e6-9910-4170-921d-13db4f1da6cb" />

<img width="768" height="135" alt="Image" src="https://github.com/user-attachments/assets/147b773f-6f16-462f-8ac1-471a148ee91e" />

### 2-8 페이지 완독률 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
SELECT
  url,
  action,
  COUNT(1) AS count,
  100.0 * COUNT(1) / SUM(CASE WHEN action = 'view' THEN COUNT(1) ELSE 0 END) OVER(PARTITION BY url) AS action_per_view
FROM
  read_log
GROUP BY
  url,
  action
ORDER BY
  url,
  count DESC;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="665" height="527" alt="Image" src="https://github.com/user-attachments/assets/9679c08f-daf4-4460-a566-0c77bfb94ac3" />

### 2-9 사용자 행동 전체를 시각화하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

<!-- 
```sql
여기에 코드를 적어주세요.
``` 
-->

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

## 3. 입력 양식 최적화하기 

### 3-1 오류율 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
SELECT
  COUNT(*) AS confirm_count,
  SUM(CASE WHEN status = 'error' THEN 1 ELSE 0 END) AS error_count,
  AVG(CASE WHEN status = 'error' THEN 1.0 ELSE 0.0 END) AS error_rate,
  SUM(CASE WHEN status = 'error' THEN 1.0 ELSE 0.0 END) / COUNT(DISTINCT session) AS error_per_user
FROM
  form_log
WHERE
  path = '/regist/confirm';
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="366" height="174" alt="Image" src="https://github.com/user-attachments/assets/e6d4e575-6b2f-4c0d-b23a-86afb90a8ea9" />

### 3-2 입력 ~ 확인 ~ 완료까지의 이동률 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH mst_fallout_step AS (
  SELECT 1 AS step, '/regist/input' AS path
  UNION ALL SELECT 2 AS step, '/regist/confirm' AS path
  UNION ALL SELECT 3 AS step, '/regist/complete' AS path
),
form_log_with_fallout_step AS (
  SELECT
    l.session,
    m.step,
    m.path,
    MAX(l.stamp) AS max_stamp,
    MIN(l.stamp) AS min_stamp
  FROM
    mst_fallout_step AS m
  JOIN
    form_log AS l
    ON m.path = l.path
  WHERE
    status = ''
  GROUP BY
    l.session, m.step, m.path
),
form_log_with_mod_fallout_step AS (
  SELECT
    session,
    step,
    path,
    max_stamp,
    LAG(min_stamp) OVER(PARTITION BY session ORDER BY step) AS lag_min_stamp,
    MIN(step) OVER(PARTITION BY session) AS min_step,
    COUNT(1) OVER(PARTITION BY session ORDER BY step ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS cum_count
  FROM
    form_log_with_fallout_step
),
fallout_log AS (
  SELECT
    session,
    step,
    path
  FROM
    form_log_with_mod_fallout_step
  WHERE
    min_step = 1 AND
    step = cum_count AND
    (lag_min_stamp IS NULL OR max_stamp >= lag_min_stamp)
)
SELECT
  step,
  path,
  COUNT(1) AS count,
  100.0 * COUNT(1) / FIRST_VALUE(COUNT(1)) OVER(ORDER BY step ASC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS first_trans_rate,
  100.0 * COUNT(1) / LAG(COUNT(1)) OVER(ORDER BY step ASC) AS step_trans_rate
FROM
  fallout_log
GROUP BY
  step, path
ORDER BY
  step;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="363" height="344" alt="Image" src="https://github.com/user-attachments/assets/93651dca-b441-4bb8-98ad-5cdb10cfa627" />

### 3-3 입력 양식 직귀율 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH form_with_progress_flag AS (
  SELECT
    substring(stamp, 1, 10) AS dt,
    session,
    SIGN(SUM(CASE WHEN path IN ('/regist/input') THEN 1 ELSE 0 END)) AS has_input,
    SIGN(SUM(CASE WHEN path IN ('/regist/confirm', '/regist/complete') THEN 1 ELSE 0 END)) AS has_progress
  FROM
    form_log
  GROUP BY
    dt, session
)
SELECT
  dt,
  COUNT(1) AS input_count,
  SUM(CASE WHEN has_progress = 0 THEN 1 ELSE 0 END) AS bounce_count,
  100.0 * AVG(CASE WHEN has_progress = 0 THEN 1 ELSE 0 END) AS bounce_rate
FROM
  form_with_progress_flag
WHERE
  has_input = 1
GROUP BY
  dt;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="625" height="74" alt="Image" src="https://github.com/user-attachments/assets/5c77db28-5117-4f22-a962-119af5030fdb" />

### 3-4 오류가 발생하는 항목과 내용 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
SELECT
  form,
  field,
  error_type,
  COUNT(1) AS count,
  100.0 * COUNT(1) / SUM(COUNT(1)) OVER(PARTITION BY form) AS share
FROM
  form_error_log
GROUP BY
  form,
  field,
  error_type
ORDER BY
  form,
  count DESC;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="711" height="278" alt="Image" src="https://github.com/user-attachments/assets/f1c77f9e-1fc1-415d-a7bd-058c92d3fb80" />

### 🎉 수고하셨습니다.
