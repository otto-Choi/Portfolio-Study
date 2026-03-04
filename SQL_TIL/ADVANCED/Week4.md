# SQL_ADVANCED 4주차 정규 과제 

## Week 4 : TOP-N 쿼리

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스, SolveSql, LeetCode 중에서 SQL 문제 4문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_4th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_4th

### 15.2.13. SELECT Statement

- `ORDER BY, LIMIT, LIMIT and Subqueries` 중심으로 학습해주세요. 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 1주차 | 서브쿼리 & CTE          | ✅         |
| 2주차 | 집합 연산자 & 그룹 함수 | ✅         |
| 3주차 | 윈도우 함수             | ✅         |
| 4주차 | Top N 쿼리              | ✅         |
| 5주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 6주차 | PIVOT / UNPIVOT         | 🍽️         |
| 7주차 | 정규 표현식             | 🍽️         |



### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**



# 1️⃣ 학습 내용

> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 15.2.13 SELECT Statement : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/select.html#order-by-optimization
>
> (한국어 버전) https://dart-b-official.github.io/posts/mysql-TopN/
>



<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 2️⃣ 학습 내용 정리하기

## 1. TOP N 쿼리

~~~
✅ 학습 목표 :
* LIMIT 와 ORDER BY를 이용한 TOP-N 쿼리 작성이 가능하다.
* SubQuery나 RANK 대신 LIMIT으로 간단한 순위 집계가 가능함을 이해한다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

특정 데이터가 최댓값이거나 최솟값인 행을 찾아 출력할 때, ORDER BY와 LIMIT을 사용한다.<br>
예를 들어,
~~~sql
SELECT *
FROM ex_table
ORDER BY Salary DESC
LIMIT 1
~~~
를 통해 급여가 가장 큰 행의 정보를 출력한다거나,
~~~sql
SELECT *
FROM ex_table
ORDER BY Num_Queue
LIMIT 5
~~~
를 통해 대기열 숫자가 가장 작은 5개의 행을 출력할 수도 있다.

이러한 기능에 대해선 알고 있었으나, 새로 배운 내용은 LIMIT에 여러 수치를 입력했을때의 기능이다.<br>
LIMIT n,m 을 입력하는 경우 n+1의 순서부터 m개의 행을 출력한다.




<br>

<br>

---

# 3️⃣ 실습 문제

## 문제 

https://leetcode.com/problems/rank-scores/

> LeetCode 178. Rank Scores
>
> 학습 포인트 : DENSE_RANK( )를 활용하여 점수별 순위 부여, 동점자 처리, 윈도우 함수 복습 

~~~sql
SELECT
    score,
    Dense_rank() over(order by score DESC) AS 'RANK'
FROM Scores;
~~~
다른 모든 부분은 쉽게 작성했으나, 윈도우 함수 중 rank 중에서도 타이브레이커에 대한 세부적인 기능을 가진 함수가 기억나지 않아 구글에 검색해서 발견했다.<br>
또한, 별 생각 없이 AS RANK 로 작성했다가 에러가 발생해 AS 'RANK'로 수정했다.

--- 

https://school.programmers.co.kr/learn/courses/30/lessons/133027

> 프로그래머스 : 주문량이 많은 아이스크림들 조회하기 (Lev 4)
>
> Hint
>
> - 문제 핵심은 '총 주문량 합산' 입니다. 
>
> - 두 테이블을 '세로로' 합쳐야합니다. 
>   - 저희는 이 부분을 1주차에 `UNION ALL` 을 통해 방법을 배웠습니다. 
> - 합쳐진 테이블에서 FLAVOR 별로 그룹화해 주문량을 합산하세요. 
> - 상위 3개를 추출 = 주문량 기준으로 내림차순하여 이번에 학습한 것을 사용해야 합니다. 


~~~sql
SELECT
    FLAVOR
FROM (SELECT *
FROM FIRST_HALF
UNION
SELECT *
FROM JULY)
FIRST_HALF
GROUP BY FLAVOR
ORDER BY SUM(TOTAL_ORDER) DESC
LIMIT 3;
~~~
작성에 가장 어려움을 겪은 부분은 UNION 절이다. 처음엔 SELECT 바로 아래 UNION TABLE ON을 덧붙여 작성했으나, 에러가 발생해 Week 2 내용을 다시 읽어보고 수정했다.<br>
서브쿼리를 활용해서 우선 이어 붙인 후에 GROUP BY, ORDER BY를 적용하니 쉽게 해결됐다.

---

## 문제 인증란

<!-- 이 주석을 지우고 여기에 문제 푼 인증사진을 올려주세요. -->

<img width="784" height="552" alt="image" src="https://github.com/user-attachments/assets/aa276b51-488c-4325-b690-6d272f9cbe6a" />

<img width="766" height="349" alt="image" src="https://github.com/user-attachments/assets/eacd2bdc-22aa-421a-9e65-74565e38fd21" />


---

# 확인문제

## 문제 1

> **🧚영리는 지역별로 가장 인기 있는 식당 2곳씩을 뽑기 위해 다음과 같은 UNION ALL 기반 쿼리를 작성했습니다.**

~~~sql
(
  SELECT region, restaurant_name, review_count
  FROM Restaurants
  WHERE region = '서울'
  ORDER BY review_count DESC
  LIMIT 2
)
UNION ALL
(
  SELECT region, restaurant_name, review_count
  FROM Restaurants
  WHERE region = '부산'
  ORDER BY review_count DESC
  LIMIT 2
)
UNION ALL
(
  SELECT region, restaurant_name, review_count
  FROM Restaurants
  WHERE region = '대구'
  ORDER BY review_count DESC
  LIMIT 2
);
~~~

> **쿼리는 잘 작동하긴 하지만, 지역을 더 추가해달라는 재원이의 부탁으로 UNION ALL 블록을 계속 추가하게 되어 관리가 어려울 것 같아서 힘들어하고 있었습니다. 여러분들은 이 쿼리를 윈도우 함수로 변경하여 더 쉽게 리팩토링을 하려고 합니다. 민서를 도와서 UNION ALL 없이 RANK( ) 또는 ROW_NUMBER( ) 윈도우 함수를 사용해, 각 지역별로 리뷰 수가 가장 많은 상위 2개 식당을 추출하는 쿼리를 작성해보세요.**


첫 답안
~~~sql
SELECT region, restaurant_name, review_count
FROM Restaurants
WHERE RANK() OVER(PARTITION BY region ORDER BY review_count) <= 2
ORDER BY region, review_count DESC
~~~

2번째 답안
~~~sql
SELECT t.region, t.restaurant_name, t.review_count
FROM (
  SELECT *,
    ROW_NUMBER() OVER(PARTITION BY region ORDER BY review_count DESC, restaurant_id) AS rk
  FROM Restaurants
) AS t
WHERE t.rk <= 2
ORDER BY region, review_count DESC, restaurant_id
~~~

우선 서브쿼리를 이용했다. 윈도우 함수는 WHERE에서 사용할 수 없기에 서브쿼리를 통해 활용되는 데이터에 윈도우 함수 결과값을 추가했다.<br>
그리고 ROW_NUMBER()함수를 사용했다. RANK 함수 사용시 지역별로 2개 이상의 결과값이 나올 수 있기 때문이다.<br>
또한 rk, t와 같은 참조를 위한 식별자를 사용했다.<br>
마지막으로 restaurant_id를 정렬 기준에 추가해 같은 결과값이 나오도록 했다.



<br>

### 🎉 수고하셨습니다.
