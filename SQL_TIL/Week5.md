# SQL_BASIC 5주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_5th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**5주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **3 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 4문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 



## SQL_BASIC_5th

### 섹션 5. 데이터 탐색 - 변환

### 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

### 4-5. 시간 데이터 연습문제 1~2번

### 4-5. 시간 데이터 연습문제 3~5번

### 4-6. 조건문 (CASE WHEN, IF)

### 4-7. 조건문 연습 문제

### 4-8. 정리

### 4-9. BigQuery 공식 문서 확인하는 법

(강의에서 연습문제가 많아서 따로 프로그래머스 문제 과제는 없습니다.)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>



<!-- 여기까진 그대로 둬 주세요-->

---

# 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터에 대해서 더 자세히 설명할 수 있다. 
* CURRENT_TIME, EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME 을 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

날짜 및 시간 데이터 타입
- DATE
- DATETIME: DATE + TIME : 타임존 정보 X
- TIMESTAMP: 특정 시점에 도장찍은 값으로, 타임존 정보 포함
- UTC: 국제적인 표준 시간. 한국은 UTC+9
- Milisecond: 1/1000초
- Microsecond: 1/1000ms

---

CURRENT_DATETIME([time_zone]): 현재 DATETIME 출력


EXTRACT(part FROM datetime_expression): DATETIME에서 특정 부분만 추출하고 싶은 경우

DATETIME_TRUNC(datetime_expression, date_time_part): 특정 유형을 기준으로 시간을 자르기

PARSE_DATETIME('문자열 형태', datetime_expression): 문자열에 저장된 DATETIME을 DATETIME 타입으로 바꾸고 싶은 경우

FORMAT_DATETIME("%c", datetime_expression): DATETIME 타입 데이터를 특정 형태의 문자열 데이터로 변환하고 싶은 경우

LAST_DAY(DATETIME): 월의 마지막 값을 반환. 인자를 추가하여 다른 값을 얻을 수도 있음.

DATETIME_DIFF(첫 DATETIME, 두번째 DATETIME, 궁금한 차이): 두 DATETIME의 차이를 알고 싶은 경우




# 4-6. 조건문(CASE WHEN, IF)

~~~
✅ 학습 목표 :
* 조건문 함수의 기능을 이해하고, 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

CASE WHEN 
~~~sql
CASE
   WHEN 조건1 THEN 결과1
   WHEN 조건2 THEN 결과2
   ELSE 그 외 결과
END AS 컬럼명
~~~
엑셀에서 IF문을 활용하는 것을 그대로 적용한다.<br>
숫자 등을 비교하는 조건문을 작성할 때 겹치는 부분이 먼저 작성되지 않도록 주의하여야 한다.

---

IF

IF(조건문, True일 때의 값, False일 때의 값) AS 컬럼명

더욱 간단하게 사용 가능하다. 그러나 여러 조건들을 사용하게 되면 복잡해지기 시작한다. 따라서 단일 조건일때만 사용한다.


 # 4-5. 시간 데이터 연습문제 & 4-7. 조건문 연습 문제

~~~
✅ 학습 목표 :
* 4-5, 4-7 각각에서 두 문제 이상 (최소 4문제) 푼 내용 정리하기
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

1번. 트레이너가 포켓몬을 포획한 날짜(catch_date)를 기준으로, 2023년 1월에 포획한 포켓몬의 수를 계산해주세요.<br>
답안
~~~sql
SELECT
  COUNT(*) as cnt
FROM basic.trainer_pokemon
WHERE catch_date between '2023-01-01' AND '2023-01-31';
~~~
출력값: 86


정답
~~~sql
SELECT
  COUNT(DISTINCT id) AS cnt
FROM basic.trainer_pokemon
WHERE
  EXTRACT(YEAR FROM DATETIME(catch_datetime, "Asia/Seoul")) = 2023
  AND EXTRACT(MONTH FROM DATETIME(catch_datetime, "Asia/Seoul")) = 1;
~~~
출력값: 85

DATETIME을 검토하는 과정이 필요했다. 뿐만 아니라 해당 강의에서 알게 된 내용을 활용하고자 해야겠다.

---
2번. 배틀이 일어난 시간(battle_datetime)을 기준으로, 오전 6시에서 오후 6시 사이에 일어난 배틀의 수를 계산해주세요.

답안 및 정답
~~~sql
SELECT
  COUNT(DISTINCT id) AS cnt
FROM basic.battle
WHERE
  EXTRACT(HOUR FROM battle_datetime) >= 6
  AND EXTRACT(HOUR FROM battle_datetime) <=18;
~~~

---
3번. 각 트레이너별로 그들이 포켓몬을 포획한 첫 날(catch_date)을 찾고, 그 날짜를 DD/MM/YYYY 형식으로 출력해 주세요.

답안
~~~sql
SELECT
  b.name AS Name,
  FORMAT_DATETIME("%d/%m/%Y",DATETIME(catch_datetime, "Asia/Seoul")) AS FIRT_DAY
FROM (
  SELECT
    trainer_id,
    catch_datetime,
    ROW_NUMBER() OVER(PARTITION BY trainer_id ORDER BY catch_datetime) AS rn
  FROM basic.trainer_pokemon
) AS a
LEFT JOIN basic.trainer AS b
  ON a.trainer_id = b.id
WHERE rn = 1;
~~~

정답
~~~sql
SELECT
  trainer_id,
  FORMAT_DATE("%d/%m/%Y", min_catch_date) AS new_min_catch_date
FROM(
  SELECT
    trainer_id,
    MIN(DATE(catch_datetime, "Asia/Seoul"))AS min_catch_date
  FROM basic.trainer_pokemon
  GROUP BY trainer_id
)
ORDER BY trainer_id
~~~
MIN()을 이용해 훨씬 쉽게 최초의 날짜를 파악할 수 있다.

---
4번. 배틀이 일어난 날짜(battle_date)를 기준으로, 요일별로 배틀이 얼마나 자주 일어났는지 계산해주세요.

답안
~~~sql
SELECT
  weekday,
  COUNT(DISTINCT id) as cnt
FROM (
  SELECT
    *,
    EXTRACT(DAYOFWEEK FROM battle_datetime) AS weeknum,
    CASE
      WHEN
        EXTRACT(DAYOFWEEK FROM battle_datetime) = 1 THEN '일요일'
      WHEN
        EXTRACT(DAYOFWEEK FROM battle_datetime) = 2 THEN '월요일'
      WHEN
        EXTRACT(DAYOFWEEK FROM battle_datetime) = 3 THEN '화요일'
      WHEN
        EXTRACT(DAYOFWEEK FROM battle_datetime) = 4 THEN '수요일'
      WHEN
        EXTRACT(DAYOFWEEK FROM battle_datetime) = 5 THEN '목요일'
      WHEN
        EXTRACT(DAYOFWEEK FROM battle_datetime) = 6 THEN '금요일'
      WHEN
      EXTRACT(DAYOFWEEK FROM battle_datetime) = 7 THEN '토요일'
    END AS weekday
  FROM basic.battle)
GROUP BY weekday
~~~
우선, ORDER BY를 적절히 사용하지 못했다. 서브쿼리 내에서 스트링 문자열로 요일을 기록하여 이를 정렬하는 것에 어려움을 겪었다.

정답
~~~sql
SELECT
  day_of_week,
  COUNT(DISTINCT id) AS battle_cnt
FROM (
  SELECT
    *,
    EXTRACT(DAYOFWEEK FROM battle_date) AS day_of_week
  FROM basic.battle
)
GROUP BY day_of_week
ORDER BY day_of_week;
~~~
데이터셋 활용 시 날짜 데이터를 그대로 활용할 수 있는지 확인하고 괜찮다면 제시된 컬럼을 그대로 사용한다.

---

5번. 트레이너가 포켓몬을 처음으로 포획한 날짜와 마지막으로 포획한 날짜의 간격이 큰 순으로 정렬하는 쿼리를 작성해주세요.

답안
~~~sql
SELECT
  b.name,
  a.diff
FROM (
  SELECT
    trainer_id,
    DATETIME_DIFF(MAX(DATETIME(catch_datetime, "Asia/Seoul")),MIN(DATETIME(catch_datetime, "Asia/Seoul")), DAY) as diff
  FROM basic.trainer_pokemon
  GROUP BY trainer_id
) AS a
LEFT JOIN basic.trainer b
  ON a.trainer_id = b.id
ORDER BY a.diff DESC;
~~~

정답
~~~sql
SELECT
  *,
  DATETIME_DIFF(max_catch_datetime, min_catch_datetime, DAY) AS diff
FROM(
  SELECT
    trainer_id,
    MIN(DATETIME(catch_datetime, "Asia/Seoul")) AS min_catch_datetime,
    MAX(DATETIME(catch_datetime, "Asia/Seoul")) AS min_catch_datetime
  FROM basic.trainer_pokemon
  GROUP BY trainer_id
)
ORDER BY diff DESC;
~~~
큰 차이는 없으나, 최소 날짜와 최대 날짜를 별도의 열으로 저장하는 과정을 거쳤다.

---

1번. 포켓몬의 'Speed'가 70 이상이면 '빠름', 그렇지 않으면 '느림'으로 표시하는 새로운 컬럼 'Speed_Category'를 만들어 주세요.

답안
~~~sql
SELECT
  *,
  CASE
    WHEN Speed >= 70 THEN '빠름'
    ELSE '느림'
  END AS Speed_Category
FROM basic.pokemon;
~~~

정답
~~~SQL
SELECT
  *,
  IF(Speed >= 70, '빠름', '느림') AS Speed_Category
FROM basic.pokemon;
~~~
단일 조건문이므로 IF문 사용시 훨씬 간편하다.

---
2번. 포켓몬의 'type1'에 따라 'Water', 'Fire', 'Electric' 타입은 각각 '물, '불', '전기'로, 그 외 타입은 '기타'로 분류하는 새로운 컬럼 'type_korean'을 만들어 주세요.

답안 및 정답
~~~sql
SELECT
  *,
  CASE
    WHEN type1 = 'Water' THEN '물'
    WHEN type1 = 'Fire' THEN '불'
    WHEN type1 = 'Electric' THEN '전기'
    ELSE '기타'
  END AS type_korean
FROM basic.pokemon;
~~~

---

3번. 각 포켓몬의 총점(total)을 기준으로, 300 이하면 'Low', 301에서 500 사이면 'Medium', 501 이상이면 'High'로 분류해 주세요.

답안
~~~sql
SELECT
  *,
  CASE
    WHEN total <= 300 THEN 'Low'
    WHEN total >= 501 Then 'High'
    ELSE 'Medium'
  END AS total_cluster
FROM basic.pokemon;
~~~

---
4번, 5번. 위 문제들과 표시만 다른 동일한 문제이기 때문에 넘어갔다.

---
6번. 배틀에서 승자(winner_id)가 player1_id와 같으면 'Player 1 Wins', Player2_id와 같으면 'Player 2 Wins', 그렇지 않으면 'Draw'로 결과가 나오게 해주세요.

답안 및 정답
~~~sql
SELECT
  *,
  CASE
    WHEN winner_id = player1_id THEN 'Player 1 Wins'
    WHEN winner_id = player2_id THEN 'Player 2 Wins'
    ELSE 'Draw'
  END AS who_win
FROM basic.battle;
~~~
 

# 학습 인증란
<!-- 이 글을 지우고, 여기에 학습한 것을 인증해주세요.-->


<br>

<br>

---

# 확인문제

## 문제 1

> **🧚Q. 광윤이는 사용자 로그 데이터에서, 2021년에 접속한 사용자 수를  집계하려고 했습니다. 그는 여러 SQL 쿼리들을 실행해봤지만, 그 중 일부는 문법적으로 잘못되어 실행되지 않았습니다. 다음 보기 중 틀린 쿼리를 모두 골라보세요 (복수 선택 가능)**

~~~sql
1. SELECT COUNT(*)  
   FROM user_log  
   WHERE EXTRACT(YEAR FROM login_date) = 2021;

2. SELECT EXTRACT(YEAR FROM login_date), COUNT(*)  
   FROM user_log  
   GROUP BY EXTRACT(YEAR FROM login_date);

3. SELECT COUNT(*)  
   FROM user_log  
   WHERE login_date = '2021';

4. SELECT COUNT(*)  
   FROM user_log  
   WHERE login_date BETWEEN '2021-01-01' AND '2021-12-31';
~~~

<!-- 틀린쿼리에 대한 오류의 원인도 같이 작성해주세요. 문제에서 제공된 login_data 컬럼은 DATE type의 데이터를 가지고 있다고 가정하시면 됩니다. -->

~~~
틀린 쿼리: 3
~~~
login_date는 DATE type 데이터인 반면 '2021'은 문자열이라 유효하지 않다.



## 문제 2

> **🧚Q. 혜성이는 포켓몬 타입에 따라 설명을 부여하는 쿼리를 작성했습니다. type 1 컬럼의 값에 따라 조건을 분기했으며, 다음 SQL 쿼리를 실행했습니다.**

~~~sql
SELECT name,
       CASE 
         WHEN type1 = 'Fire' THEN 'Hot'
         WHEN type1 = 'Water' THEN 'Cool'
         ELSE 'Normal'
       END AS type_description
FROM pokemon;
~~~

> **다음 중 type_description의 결과가 'Normal'로 출력될 포켓몬은?**

| **name**   | **type1** |
| ---------- | --------- |
| Pikachu    | Electric  |
| Charmander | Fire      |
| Squirtle   | Water     |
| Bulbasaur  | Grass     |

<!-- 근거와 함께 답을 작성해주세요 -->

~~~
Pikachu, Bulbasaur
~~~
WHEN 으로 조건이 정의된 Fire, Water을 type1의 값으로 가지는 행 외의 데이터는 Normal의 값을 가진다.


<br>

### 🎉 수고하셨습니다.
