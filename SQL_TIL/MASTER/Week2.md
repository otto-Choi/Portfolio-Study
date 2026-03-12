# SQL_MASTER 2주차 정규과제

📌SQL MASTER 정규과제는 매주 정해진 분량의 『*데이터 분석을 위한 SQL 레시피*』 를 읽고 학습하는 것입니다. 이번 주는 아래의 **SQL_MASTER_2st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래 실습을 수행하며 학습 내용을 직접 적용해보세요. 단순히 결과를 재현하는 것이 아니라, SQL을 직접 작성하는 과정에서 개념을 스스로 정리하는 것이 중요합니다.

필요한 경우 교재와 추가 자료를 참고하여 이해를 보완하시기 바랍니다.

## SQL_MASTER_2st_TIL

### 3장 데이터 가공믈 위한 SQL
#### 1. 하나의 값 조작하기
#### 2. 여러 개의 값에 대한 조작
#### 3. 하나의 테이블에 대한 조작
#### 4. 여러 개의 테이블 조작하기


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.20~50    | ✅         |
| 2주차 | p.52~136   | ✅         |
| 3주차 | p.138~184  | 🍽️         |
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

## 1. 하나의 값 조작하기 

### 1-1 코드 값을 레이블로 변경하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->
너무 오랜만에 쿼리를 작성해서 어떻게 해야 하나 싶었는데, 조금씩 떠오름.<br>
CASE문엔 WHEN과 THEN, END 및 AS를 사용함.

```sql
SELECT
    user_id,
    CASE 
        WHEN register_device =1 THEN '데스크톱'
        WHEN register_device =2 THEN '스마트폰'
        WHEN register_device =3 THEN '애플리케이션'
        -- ELSE  ''
    END AS device_name
FROM mst_users;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="349" height="141" alt="image" src="https://github.com/user-attachments/assets/74efeb33-d4a6-41ea-a4c3-233484b369cb" />


### 1-2 URL에서 요소 추출하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

이 부분에서 MYSQL과 PostgreSQL의 차이를 느끼고 PostgreSQL 환경을 다시 설정했다.

'https?://([^/]*)' 이 부분을 이해할 수 없어 추가적으로 찾아봤다.

|부분|의미|
|---|---|
|http|문자 그대로|
|s?|s키의 유무 관계 없이 매칭|
|://|:// 그대로|
|( )|괄호 안의 내용을 추출함|
|[^/]|'/'가 아닌 모든 문자 추출|
|*|앞 문자가 0개 이상(없어도 됨)|
|+|앞 문자가 1개 이상(최소 1개)|
|substring|문자열의 일부 추출<br>from 사용시 정규식으로 추출하는 것임|

```sql
SELECT
    stamp,
    substring(referrer from 'https?://([^/]*)') AS referrer_host
FROM access_log;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="362" height="134" alt="image" src="https://github.com/user-attachments/assets/76c37468-ded3-468c-a6d7-1afcd1995ac9" />

### 1-3 문자열을 배열로 분해하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
SELECT
    stamp,
    url,
    substring(url from '//[^/]+([^?#]+)') AS path,
    substring(url from 'id=([^&]*)') AS id
FROM access_log;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="593" height="139" alt="image" src="https://github.com/user-attachments/assets/2fc131b3-146f-4adb-8c35-fc8305f60b7e" />

<img width="583" height="138" alt="image" src="https://github.com/user-attachments/assets/2d4e61d4-691a-4703-9e41-1dd510cbbc5a" />

### 1-4 날짜와 타임스탬프 다루기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

만약 타임존을 적용하고 싶지 않은 경우 LOCALTIMESTAMP를 사용해야 함.

current_timestamp의 값을 텍스트로 가져오려면 ::TEXT를 붙여야 함.

```sql
SELECT
    CURRENT_DATE as dt,
    stamp,
    substring(stamp, 1, 4) as year,
    substring(stamp, 6, 2) as month,
    substring(stamp, 9, 2) as day,
    substring(stamp, 12, 2) as hour,
    substring(stamp, 15, 2) as minute,
    substring(stamp, 18, 2) as second,
    substring(stamp, 1, 7) as year_month
FROM (SELECT current_timestamp::TEXT AS stamp) AS subquery;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="1087" height="70" alt="image" src="https://github.com/user-attachments/assets/e7d1d5ac-e52e-418d-a730-dbf73e292917" />


### 1-5 결손 값을 디폴트 값으로 대치하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

COALESCE를 사용하여 NULL값을 특정 값으로 대치할 수 있다.

```sql
SELECT
    purchase_id,
    amount,
    coupon,
    amount - coupon AS discounted_amount1,
    amount - COALESCE(coupon, 0) AS discounted_amount2
FROM purchase_log_with_coupon;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="774" height="144" alt="image" src="https://github.com/user-attachments/assets/8d5f5a2e-2c7a-4f74-9b25-f8df501626ef" />


## 2. 여러 개의 값에 대한 조작 

### 2-1 문자열을 연결하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

띄어쓰기를 위해 중간에 ' ',를 추가함

```sql
SELECT
    user_id,
    concat(pref_name,' ',city_name) AS pref_city
FROM mst_user_location;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="402" height="134" alt="image" src="https://github.com/user-attachments/assets/4df54ae0-757c-4073-aab9-31c762d07ddb" />


### 2-2 여러 개의 값을 비교하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

SIGN: 양수인지 음수인지 여부를 알려 줌.

```sql
SELECT
    year,
    q1,
    q2,
    CASE 
        WHEN q1<q2 THEN '+'
        WHEN q1=q2 THEN ' ' 
        ELSE '-'
    END AS judge_q1_q2,
    q2-q1 AS diff_q2_q1,
    SIGN(q2-q1) AS sign_q2_q1
FROM quarterly_sales
ORDER BY year;

SELECT
    year,
    (q1+q2+q3+q4)/4 AS avg
FROM quarterly_sales
ORDER BY year;

SELECT
    year,
    (COALESCE(q1,0)+COALESCE(q2,0)+COALESCE(q3,0)+COALESCE(q4,0))/4 AS average
FROM quarterly_sales
ORDER BY year;

SELECT
    year,
    (COALESCE(q1,0)+COALESCE(q2,0)+COALESCE(q3,0)+COALESCE(q4,0))/(sign(COALESCE(q1,0))+sign(COALESCE(q2,0))+sign(COALESCE(q3,0))+sign(COALESCE(q4,0))) AS average
FROM quarterly_sales
ORDER BY year;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="764" height="134" alt="image" src="https://github.com/user-attachments/assets/41b11706-6f2b-42ec-94ce-bfbd7d7e70ad" />

<img width="312" height="139" alt="image" src="https://github.com/user-attachments/assets/d91f4986-999a-4fe8-8a34-bd3f69561963" />

<img width="295" height="142" alt="image" src="https://github.com/user-attachments/assets/5ce6002e-5744-4286-b64c-ca8777374d6e" />

<img width="292" height="132" alt="image" src="https://github.com/user-attachments/assets/df6ca54a-7c02-4be2-b047-d01d8d99986f" />

### 2-3 2개의 값 비율 계산하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

clicks와 impressions가 모두 정수형 타입이라 정수 나눗셈이 되어 소수점이 버려짐.<br>
따라서 100을 곱해서 계산하거나, clicks::numeric으로 변환하는 과정을 거쳐야 함.<br>
별도의 양식 지정을 안했더니 ctr_as_percentage 컬럼이 지나치게 긴 소수점까지 표시하게 되어 round를 적용함.


```sql
SELECT
    dt,
    ad_id,
    round(clicks / impressions,2) as ctr,
    round(100.0*clicks / impressions,2) as ctr_as_percentage
FROM advertising_stats
WHERE dt = '2017-04-01'
ORDER BY dt, ad_id;

SELECT
    dt,
    ad_id,
    CASE
        WHEN impressions > 0 THEN 100.0 * clicks / impressions
    END AS ctr_as_percent_by_case,
    100.0 * clicks / NULLIF(impressions, 0) AS ctr_as_percent_by_null
FROM advertising_stats
ORDER BY dt, ad_id;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="600" height="134" alt="image" src="https://github.com/user-attachments/assets/d1694210-3ef0-4419-be44-f3e5d87cdfaf" />

<img width="594" height="136" alt="image" src="https://github.com/user-attachments/assets/ecb4a2b6-bd41-4557-bad1-100b39faffb8" />

<img width="638" height="235" alt="image" src="https://github.com/user-attachments/assets/0fb3c239-d664-483f-b611-de4bb1a2972f" />

### 2-4 두 값의 거리 계산하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
SELECT
    abs(x1-x2) AS abs,
    sqrt(power(x1-x2, 2)) AS rms
FROM location_1d;

SELECT
    SQRT(POWER(x1-x2, 2) + POWER(y1-y2, 2)) AS dist
FROM location_2d
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="245" height="209" alt="image" src="https://github.com/user-attachments/assets/a4a0f9be-2940-4510-be8a-dd2a8ce9e4aa" />

<img width="204" height="135" alt="image" src="https://github.com/user-attachments/assets/50279f1a-a896-499d-bee3-144b838e5048" />

### 2-5 날짜/시간을 계산하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

위와 마찬가지로, ::뒤에 어떤 형태의 데이터타입인지 지정할 수 있음.

문자열로 변환하여 계산하더라도 당연히 같은 결과를 얻을 수 있음.<br>
익숙해지기 전까지는 문자열으로 변환하여 계산하는 것이 실수의 여지를 줄일 수 있다는 것에 이해할 수 있었음.

```sql
SELECT
    user_id,
    register_stamp::timestamp AS register_stamp,
    register_stamp::timestamp + '1 hour'::interval AS after_1_hour,
    register_stamp::timestamp - '30 minutes'::interval AS before_30_minutes,
    register_stamp::date AS register_date,
    (register_stamp::date + '1 day'::interval)::date AS after_1_day,
    (register_stamp::date - '1 month'::interval)::date AS before_1_month
FROM mst_users_with_dates;

SELECT
    user_id,
    CURRENT_DATE AS today,
    register_stamp::date AS register_date,
    CURRENT_DATE - register_stamp::date AS diff_days
FROM mst_users_with_dates;

SELECT
    user_id,
    CURRENT_DATE AS today,
    register_stamp::date AS register_date,
    birth_date::date AS birth_date,
    EXTRACT(YEAR FROM age(birth_date::date)) AS current_age,
    EXTRACT(YEAR FROM age(register_stamp::date, birth_date::date)) AS register_age
FROM mst_users_with_dates;

SELECT
    user_id,
    substring(register_stamp, 1, 10) AS register_date,
    birth_date,
    floor(
        (CAST(replace(substring(register_stamp, 1, 10), '-', '') AS integer)
        - CAST(replace(birth_date, '-', '') AS integer))
        / 10000
    ) AS register_age,
    floor(
        (CAST(replace(CAST(CURRENT_DATE AS text), '-', '') AS integer)
        - CAST(replace(birth_date, '-', '') AS integer))
        / 10000
    ) AS current_age
FROM mst_users_with_dates;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="662" height="610" alt="image" src="https://github.com/user-attachments/assets/fd8f79de-e714-4f5f-8c60-9376e55324c3" />

<img width="533" height="133" alt="image" src="https://github.com/user-attachments/assets/cad117b4-680b-43f8-a315-d31741aac493" />

<img width="677" height="565" alt="image" src="https://github.com/user-attachments/assets/adabe7ac-4eab-44ba-b7f4-4a3e239efe51" />

### 2-6 IP 주소 다루기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

IP주소 숫자의 의미...를 자격증 준비하며 공부했었는데, 거의 다 잊었다...

```sql
SELECT
    CAST('127.0.0.1' AS INET) < CAST('127.0.0.2' AS INET) AS lt,
    CAST('127.0.0.1' AS INET) > CAST('127.0.0.2' AS INET) AS gt;

SELECT
    CAST('127.0.0.1' AS INET) << CAST('127.0.0.0/8' AS INET) AS is_contained;

SELECT
    ip,
    CAST(split_part(ip, '.', 1) AS integer) AS ip_part_1,
    CAST(split_part(ip, '.', 2) AS integer) AS ip_part_2,
    CAST(split_part(ip, '.', 3) AS integer) AS ip_part_3,
    CAST(split_part(ip, '.', 4) AS integer) AS ip_part_4
FROM (SELECT CAST('192.168.0.1' AS text) AS ip) AS t;

SELECT
    ip,
    CAST(split_part(ip, '.', 1) AS integer) * 2^24
    + CAST(split_part(ip, '.', 2) AS integer) * 2^16
    + CAST(split_part(ip, '.', 3) AS integer) * 2^8
    + CAST(split_part(ip, '.', 4) AS integer) * 2^0
    AS ip_integer
FROM (SELECT CAST('192.168.0.1' AS text) AS ip) AS t;

SELECT
    ip,
    lpad(split_part(ip, '.', 1), 3, '0')
    || lpad(split_part(ip, '.', 2), 3, '0')
    || lpad(split_part(ip, '.', 3), 3, '0')
    || lpad(split_part(ip, '.', 4), 3, '0')
    AS ip_padding
FROM
    (SELECT CAST('192.168.0.1' AS text) AS ip) AS t;
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

<img width="213" height="67" alt="image" src="https://github.com/user-attachments/assets/fc67ee6d-fe5c-4f4a-95e9-e31e84570c17" />

<img width="150" height="69" alt="image" src="https://github.com/user-attachments/assets/abc35186-5d3d-46c9-8521-a7be020b06af" />

<img width="610" height="83" alt="image" src="https://github.com/user-attachments/assets/737161d8-f291-428a-9d8f-0dc7151a026f" />

<img width="303" height="82" alt="image" src="https://github.com/user-attachments/assets/23637dff-f7cc-43b8-8a35-8cefa88f5b4f" />

<img width="263" height="79" alt="image" src="https://github.com/user-attachments/assets/4d37949f-7ab0-417c-8b10-13733441c9d0" />

## 03. 하나의 테이블에 대한 조작 

### 3-1 그룹의 특징 잡기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

### 3-2 그룹 내부의 순서

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

### 3-3 세로 기반 데이터를 가로 기반으로 변환하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

### 3-4 가로 기반 데이터를 세로 기반으로 변환하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->


## 04. 여러 개의 테이블 조작하기

### 4-1 여러 개의 테이블을 세로로 결합하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

### 4-2 여러 개의 테이블을 가로로 정렬하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

### 4-3 조건 플래그를 0과 1로 표현하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

### 4-4 계산한 테이블에 이름 붙여 재사용하기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->

### 4-5 유사 테이블 만들기

<!-- 이 부분을 지우고 새롭게 배운 내용을 자유롭게 정리해주세요. -->

```sql
여기에 코드를 적어주세요.
```

<!-- 이 부분을 지우고 실행 결과 화면을 제출해주세요. -->



### 🎉 수고하셨습니다.
