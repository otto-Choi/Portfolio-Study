# SQL_MASTER 3주차 정규과제

📌SQL MASTER 정규과제는 매주 정해진 분량의 『*데이터 분석을 위한 SQL 레시피*』 를 읽고 학습하는 것입니다. 이번 주는 아래의 **SQL_MASTER_3st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래 실습을 수행하며 학습 내용을 직접 적용해보세요. 단순히 결과를 재현하는 것이 아니라, SQL을 직접 작성하는 과정에서 개념을 스스로 정리하는 것이 중요합니다.

필요한 경우 교재와 추가 자료를 참고하여 이해를 보완하시기 바랍니다.

## SQL_MASTER_3st_TIL

### 4장 매출을 파악하기 위한 데이터 추출
#### 1. 시계열 기반으로 데이터 집계하기
#### 2. 다면적인 축을 사용해 데이터 집계하기 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.20~50    | ✅         |
| 2주차 | p.52~136   | ✅         |
| 3주차 | p.138~184  | ✅         |
| 4주차 | p.186~232 | 🍽️         |
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


## 1. 시계열 기반으로 데이터 집계하기

### 1-1 날짜별 매출 집계하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
Select 
    dt,
    sum(purchase_amount) as total_purchase_amount,
    sum(purchase_amount)/count(order_id) as avg_purchase_amount
From purchase_log
GROUP BY dt
ORDER BY dt;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->
 
 <img width="520" height="365" alt="Image" src="https://github.com/user-attachments/assets/6fcde448-0d59-4af2-8c20-7a5e9446284d" />

### 1-2 이동평균을 사용한 날짜별 추이 보기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

OVER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW

7일간의 데이터

```sql
Select 
    dt,
    sum(purchase_amount) as total_purchase_amount,
    CASE 
        WHEN 7 = COUNT(*) OVER (ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
        THEN AVG(sum(purchase_amount)) OVER(ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
        ELSE NULL
    END AS seven_day_moving_avg_purchase_amount
From purchase_log
GROUP BY dt
ORDER BY dt;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->
 
 <img width="519" height="365" alt="Image" src="https://github.com/user-attachments/assets/7e0c637a-0a96-4a6a-af3e-a9780412ae95" />

### 1-3 당월 매출 누계 구하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

지난시간에 다시 배운 WITH절을 활용하여 반복적으로 나오는 구문을 깔끔하게 수정할 수 있음.

```sql
WITH
daily_purchase AS (
    Select 
        dt,
        sum(purchase_amount) as total_purchase_amount,
        count(order_id) as total_order_count,
        count(*) OVER (ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) as seven_day_count
    From purchase_log
    GROUP BY dt
)
SELECT
    dt,
    total_purchase_amount,
    sum(total_purchase_amount) over (order by dt rows UNBOUNDED PRECEDING) as total_total
FROM daily_purchase
ORDER BY dt;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="419" height="364" alt="Image" src="https://github.com/user-attachments/assets/2617dbdd-1105-4329-87e4-7abe41bdcc1a" />

### 1-4 월별 매출의 작대비 구하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

with를 중첩하여 사용하는 것도 편리하다. 괜히 서브쿼리 이상하게 중첩하는 것 보다 훨씬 효과적이다.<br>
LAG를 이용하여 일정 간격 전의 데이터를 매칭할 수 있다.<br>
성장률을 계산하기 전에 100.0을 곱하여 실수 처리를 먼저 해주어야 정확한 값을 얻을 수 있다.

```sql
WITH monthly_purchase AS (
    SELECT
        substring(dt, 1, 4) as year,
        substring(dt, 6, 2) as month,
        sum(purchase_amount) as purchase_amount
    FROM purchase_log
    GROUP BY year, month      
), monthly_with_prev_year AS (
    SELECT
        year,
        month,
        CONCAT(year, '-', month) as year_month,
        purchase_amount,
        LAG(purchase_amount, 12) OVER (ORDER BY year, month) AS prev_year_amount
    FROM monthly_purchase
)
SELECT
    year_month,
    purchase_amount,
    ROUND(100.0 * (purchase_amount - prev_year_amount) / prev_year_amount, 2) AS growth_rate
FROM monthly_with_prev_year
WHERE year = '2015'
ORDER BY year_month;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="452" height="426" alt="Image" src="https://github.com/user-attachments/assets/c1c2691b-a2ea-4be5-8aeb-3aeb22917459" />
 
월별 데이터 합계에 차이가 나는 듯...?

### 1-5 Z 차트로 업적의 추이 확인하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH monthly_purchase AS (
    Select 
        substring(dt, 1, 4) as year,
        substring(dt, 6, 2) as month,        
        sum(purchase_amount) as monthly_amount
    From purchase_log
    GROUP BY year, month
), purchase_z AS(
    SELECT
        year,
        month,
        monthly_amount,
        SUM(CASE WHEN year = '2015' THEN monthly_amount END) over (order by year,month rows UNBOUNDED PRECEDING) as total_amount,  
        SUM(monthly_amount) OVER (ORDER BY year,month ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS moving_amount
    FROM monthly_purchase
)
SELECT
    year,
    month,
    monthly_amount,
    total_amount,
    moving_amount
FROM purchase_z
WHERE year = '2015'
ORDER BY year,month;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->
 
<img width="670" height="426" alt="Image" src="https://github.com/user-attachments/assets/40819a2e-4a59-430e-bde3-1936e43ca11f" />

### 1-6 매출을 파악할 때 중요 포인트 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH monthly_purchase AS (
    Select 
        substring(dt, 1, 4) as year,
        substring(dt, 6, 2) as month,        
        sum(purchase_amount) as monthly_amount,
        COUNT(order_id) AS order_count
    From purchase_log
    GROUP BY year, month
), purchase_month AS(
    SELECT
        year,
        month,
        monthly_amount,
        CASE WHEN year = '2015' THEN order_count END AS count_2015,
        SUM(CASE WHEN year = '2015' THEN monthly_amount ELSE 0 END) over (order by year,month rows UNBOUNDED PRECEDING) as total_amount,  
        SUM(CASE WHEN year = '2014' THEN monthly_amount END) over (PARTITION BY month) AS amount_2014,
        SUM(CASE WHEN year = '2015' THEN monthly_amount END) over (PARTITION BY month) AS amount_2015
    FROM monthly_purchase
)
SELECT
    CONCAT(year,'년 ',month,'월'),
    count_2015,
    monthly_amount / count_2015 AS avg_amount,
    amount_2015,
    total_amount,
    amount_2014
FROM purchase_month
WHERE year = '2015'
ORDER BY year,month;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="728" height="428" alt="Image" src="https://github.com/user-attachments/assets/c3b37316-1406-4daa-85c9-9511ebc2940d" />

## 2. 다면적인 축을 사용해 데이터 집계하기 

### 2-1 카테고리별 매출과 소계 계산하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
SELECT 
    COALESCE(category, 'all') AS category,
    COALESCE(sub_category, 'all') AS sub_category,
    sum(price) AS amount
FROM purchase_detail_log
GROUP BY ROLLUP(category, sub_category)
ORDER BY category, sub_category;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="457" height="655" alt="Image" src="https://github.com/user-attachments/assets/ca5b968d-2db3-4b58-a51f-e39094629029" />

### 2-2 ABC 분석으로 잘 팔리는 상품 판별하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH purchase_category AS(
SELECT    
    category,
    sum(price) AS category_amount
FROM purchase_detail_log
GROUP BY category
), purchase_ratio AS(
SELECT 
    category,
    category_amount,
    100.0* category_amount / sum(category_amount) OVER() AS ratio,
    100.0* SUM(category_amount) OVER (ORDER BY category_amount DESC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) / sum(category_amount) OVER() AS ratio_stacked
FROM purchase_category
)
SELECT *,
    CASE 
        WHEN ratio_stacked BETWEEN 0 AND 70 THEN 'A'
        WHEN ratio_stacked BETWEEN 70 AND 90 THEN 'B'
        WHEN ratio_stacked BETWEEN 90 AND 100 THEN 'C'
    END AS rank
FROM purchase_ratio
ORDER BY category_amount DESC;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="724" height="297" alt="Image" src="https://github.com/user-attachments/assets/3f0f98e6-401b-44ba-8db9-99ec81871ea9" />

### 2-3 팬 차트로 상품의 매출 증가율 확인하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH daily_purchase_category AS (
    SELECT
        dt,
        category,
        substring(dt, 1, 4) AS year,
        substring(dt, 6, 2) AS month,
        substring(dt, 9, 2) AS date,
        SUM(price) AS category_amount
    FROM purchase_detail_log
    GROUP BY dt, category
), monthly_purchase_category AS (
    SELECT
        concat(year, '-', month) AS year_month,
        category,
        SUM(category_amount) AS category_amount
    FROM daily_purchase_category
    GROUP BY year, month, category
)
SELECT
    year_month,
    category,
    category_amount,
    FIRST_VALUE(category_amount) OVER(PARTITION BY category ORDER BY year_month ROWS UNBOUNDED PRECEDING) AS base_amount,
    100.0 * category_amount / FIRST_VALUE(category_amount) OVER(PARTITION BY category ORDER BY year_month ROWS UNBOUNDED PRECEDING) AS rate
FROM monthly_purchase_category
ORDER BY year_month, category;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="653" height="292" alt="Image" src="https://github.com/user-attachments/assets/21310621-1277-4403-a412-235ea27e9db4" />

### 2-4 히스토그램으로 구매 가격대 집계하기 

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
WITH stats AS (
    SELECT
        MAX(price) + 1 AS max_price,
        MIN(price) AS min_price,
        MAX(price) + 1 - MIN(price) AS range_price,
        10 AS bucket_num
    FROM purchase_detail_log
), purchase_log_with_bucket AS (
    SELECT
        price,
        min_price,
        range_price,
        bucket_num,
        price - min_price AS diff,
        FLOOR(
            1.0 * (price - min_price)
            / (1.0 * range_price / bucket_num)
        ) + 1 AS bucket
    FROM purchase_detail_log, stats
) SELECT
    bucket,
    MIN(price) AS lower_limit,
    MAX(price) AS upper_limit,
    COUNT(price) AS num_purchase,
    SUM(price) AS total_amount
FROM purchase_log_with_bucket
GROUP BY bucket
ORDER BY bucket;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="674" height="271" alt="Image" src="https://github.com/user-attachments/assets/aac4e23e-a57c-4078-b4f3-f4170e3eb257" />

### 🎉 수고하셨습니다.

어려워요ㅠㅠ<br>
추가로, 데이터셋의 기간 및 볼륨이 교재와 다른 상태..?