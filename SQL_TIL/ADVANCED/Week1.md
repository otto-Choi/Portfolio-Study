# SQL_ADVANCED 1주차 정규 과제 

## Week 1 : 서브쿼리 & CTE

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_0th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**👀 (수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_1st_TIL 

### 15.2.15. SubQueries

#### 특히 15.2.15.1 ~ 15.2.15.7 (Scalar, EXISTS, Correlated, Derived 등) 

### 15.2.20 WITH (Common Table Expressions)

- `WITH RECURSIVE`에 대한 내용은 추후에 공부합니다. 해당 링크에서 `WITH`에 해당하는 부분만 정리해보세요. 




## 🏁 주차별 학습 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 1주차 | 서브쿼리 & CTE          | ✅         |
| 2주차 | 집합 연산자 & 그룹 함수 | 🍽️         |
| 3주차 | 윈도우 함수             | 🍽️         |
| 4주차 | Top N 쿼리              | 🍽️         |
| 5주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 6주차 | PIVOT / UNPIVOT         | 🍽️         |
| 7주차 | 정규 표현식             | 🍽️         |

<br>


### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**



# 1️⃣ 학습 내용 

> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - SubQueries : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/subqueries.html
>
> (한국어 버전)
> https://dart-b-official.github.io/posts/mysql-subqueries/


> - CTE(공통 테이블 표현식) : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/with.html
>
> (한국어 버전)
> https://dart-b-official.github.io/posts/mysql-cte/

<br>
<br>
<!-- 여기까진 그대로 둬 주세요-->





# 2️⃣ 학습 내용 정리하기

---

 # 1. 서브쿼리

~~~
✅ 학습 목표 :
* SubQueries에 대한 문법을 이해하고 활용할 수 있다.  
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->
새로 알게 된 구문
- ANY: 서브쿼리에서 반환된 값 들 중 하나라도 조건을 만족하면 TRUE를 반환한다.
- IN: ANY의 별칭이지만, 완전히 동일한 것은 아니다. ANY가 서브쿼리에만 사용 가능한 것과 달리 IN은 표현식 리스트에도 사용 가능하다.
- SOME: ANY의 별칭으로, 동일한 기능을 가지지만 해석상 혼동을 줄이기 위해 사용된다.
- ALL: 서브쿼리에서 반환된 모든 값에 대해 비교 결과가 TRUE일 때 TRUE를 반환한다.
- NOT IN: <>ALL의 별칭이다.
- ROW: 하나의 행을 반환한다. 이 행은 여러 개의 컬럼 값을 포함할 수 있다.
- EXISTS: 서브쿼리가 한 행이라도 반환하면 TRUE를 반환한다.
- NOT EXISTS: 서브쿼리가 아무 행도 반환하지 않으면 TRUE를 반환한다.

<br><br>

서브쿼리
- 서브쿼리 결과 형태
   - Scalar: 단일 값 반환
   - Column: 단일 컬럼(여러 행 가능) 반환
   - Row: 단일 행(여러 컬럼 가능) 반환
   - Table: 다수의 행과 컬럼 반환
- 서브쿼리를 사용할 수 있는 외부 구문
   - SELECT
   - INSERT
   - UPDATE
   - SET
   - DO
- 서브쿼리의 주요 장점
   - 쿼리를 구조적으로 분리하여 각 부분을 쉽게 파악할 수 있음.
   - 복잡한 JOIN이나 UNION을 대체할 수 있는 방법 제공.
   - 많은 사람들이 복잡한 조인보다 서브쿼리를 더 읽기 쉽다고 느낌
- 상관 서브쿼리: 외부 쿼리에 있는 테이블을 참조하는 경우
   - 안쪽에서 바깥쪽 순서로 평가
   - subquery_to_derived 플래그가 켜져 있으면, 상관 스칼라 서브쿼리를 파생 테이블로 변환 가능
   - 변환 조건
      - 서브쿼리는 SELECT 리스트, WHERE, HAVING 절에는 가능
      - WHERE 절은 AND로만 연결 가능(OR 있으면 불가)
      - '=' 연산자만 가능(<=>는 불가)
      - 좌/우 항 중 하나는 내부참조, 하나는 외부 참조여야 함
      - 집계 함수 내부에는 상관 컬럼 불가
      - SELECT, JOIN, ORDER BY, GROUP BY, HAVING 절에는 상관 컬럼 포함 불가
      - 윈도우 함수 사용 불가



# 2. CTE

~~~
✅ 학습 목표 :
* CTE에 대한 문법을 이해하고 활용할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->
CTE(Common Table Expression): 공통 테이블 표현식
- 하나의 SQL문 내에서만 존재하는 이름 있는 임시 결과 집합
- 해당 문장에서 여러번 참조 가능
- 복잡한 쿼리를 구조적으로 나누고 가독성을 높이는 데 유용

CTE가 포함하는 주제
- 일반 CTE
   - CTE는 각각의 하위 쿼리 결과 집합을 이름으로 정의
   - SELECT 문에서 테이블처럼 참조 가능
   - CTE 안에서 다른 CTE를 참조 할 수 있지만, 뒤에 정의된 CTE는 참조할 수 없다.
- CTE 규칙
   - WITH 절은 SELECT, UPDATE, DELETE문 앞에 위치할 수 있다.
   - INSERT, REPLACE, CREATE TABLE/VIEW, DECLARE CURSOR, EXPLAIN문에서도 사용할 수 있다.
   - 하나의 문장 레벨에는 하나의 WITH절만 허용된다.
   - 여러 CTE는 ','로 구분해서 나열해야 한다.
   - CTE 이름은 유일해야 한다.
   - 이름 해석 순서는 서브쿼리, CTE, 기본 테이블/뷰 순서이다.
- 재귀 CTE
- 재귀 깊이 제한
- 재귀 CTE 예제
- CTE와 유사한 구조 비교
   - 파생 테이블과 비교
      - 공통점: 이름이 있고, 단일 문장에서만 존재한다.
      - 차이점
         - 파생 테이블은 한번만 참조가능한데, CTE는 여러번 참조 가능하다.
         - CTE는 자기 참조(재귀)도 가능하다.
         - 가독성 측면에선 CTE가 유리하다.






<br>

<br>

---

# 3️⃣ 실습 문제

**두 문제 중에서 한 문제는 SubQuery와 CTE를 사용한 방법을 각각 활용해서 2개의 답변을 제시해주세요**

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/131123

> 즐겨찾기가 가장 많은 식당 정보 출력하기 (GROUP BY, SubQuery) : Lev 3
첫 시도
```sql
SELECT
    R.FOOD_TYPE,
    R.REST_ID,
    R.REST_NAME,
    R.FAVORITES
FROM REST_INFO as R
    WHERE
        R.FAVORITES = (SELECT
                        FOOD_TYPE,
                        FAVORITES                           
                       FROM
                        REST_INFO
                       GROUP BY FOOD_TYPE
                      )
ORDER BY FOOD_TYPE DESC
```

오류가 발생하고 방법이 떠오르지 않아 GPT에 질문한 결과 JOIN을 활용해야 함을 인지하고 수정

```sql
SELECT
    R.FOOD_TYPE,
    R.REST_ID,
    R.REST_NAME,
    R.FAVORITES
FROM REST_INFO as R
JOIN (
    SELECT
        FOOD_TYPE,
        MAX(FAVORITES) AS MAX_FAVOR
    FROM REST_INFO
    GROUP BY FOOD_TYPE
)
AS MAX_FAV ON R.FOOD_TYPE = MAX_FAV.FOOD_TYPE AND R.FAVORITES = MAX_FAVOR
WHERE  
    R.FAVORITES = MAX_FAVOR
ORDER BY FOOD_TYPE DESC
```
알게 된 사실

조건을 설정할 떄 WHERE만을 떠올리지 말고 JOIN을 통해 소위 파생변수같은 컬럼을 만들어 활용하는 것이 필요하다.



https://school.programmers.co.kr/learn/courses/30/lessons/131115

> 가격이 제일 비싼 식품의 정보 출력하기 (SUM, MAX, MIN, SubQuery) : Lev 2

```sql
SELECT
    PRODUCT_ID,
    PRODUCT_NAME,
    PRODUCT_CD,
    CATEGORY,
    PRICE
FROM FOOD_PRODUCT
WHERE
    FOOD_PRODUCT.PRICE = (
    SELECT
        MAX(PRICE)
    FROM FOOD_PRODUCT)
```

큰 문제 없이 풀어낼 수 있었다. 위 예제 1을 풀며 마주친 시행착오 중 하나였다.



---

## 문제 인증란

<!-- 이 주석을 지우고 여기에 문제 푼 인증사진을 올려주세요. -->



---


## 문제 1

> **🧚예린이는 최근 여러 주문 데이터를 분석하는 업무를 맡게 되었습니다. 특정 고객의 주문 이력을 분석하기 위해, 다음과 같이 최근 30일간 주문만 필터링한 CTE를 사용해 쿼리를 작성했습니다.**

~~~sql
WITH RecentOrders AS (
  SELECT *
  FROM Orders
  WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
)
SELECT customer_id, COUNT(*) AS recent_order_count
FROM RecentOrders
GROUP BY customer_id;
~~~

> **그런데 예린이는 "이 쿼리를 WITH 없이, 서브쿼리 방식으로 바꿔서 실행해보라" 는 피드백을 받았고, 서브쿼리로 작성해보려 했지만 익숙하지 않아 SQL_ADVANCED를 듣는 학회원분들에게 도움을 요청하고 있습니다. 예린이의 쿼리를 WITH 없이 서브쿼리로 변환해보세요. 그리고 두 방식의 차이점을 설명해보고, 각각의 장단점을 정리해보세요**



~~~sql
SELECT 
   customer_id,
   COUNT(*)
FROM (
   SELECT *
   FROM Orders
   WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
)
GROUP BY customer_id;

~~~

CTE 방식(WITH 활용)
- 장점
   - 가독성이 좋다.
   - 정의해 둔 CTE를 메인 쿼리 내에서 여러번 참조해서 사용할 수 있다.
   - 재귀 커리를 작성할 때 필수적이다.
- 단점
   - 성능 차이가 거의 없다. 
   - CTE를 지원하지 않는 구식화된 SQL이 있을 수 있다.

서브쿼리 방식
- 장점
   - 사용이 직관적이다. 쿼리의 한 부분으로 포함되어 있어 구조를 파악하기 용이하다.
   - 구식의 CTE에서도 사용 가능하다.
- 단점
   - 가독성이 나쁘다. 서브쿼리가 여러번 중첩되는 경우 직관적으로 파악하기 어려워 질 수 있다.
   - 재사용이 불가능하다. 해당 쿼리에서만 사용된다.
   - 재귀 쿼리 작성이 불가능하다.




## 참고자료

서브쿼리를 사용하는 이유가 너무 어려우신 분들을 위해 참고자료를 첨부합니다. 아래 블로그를 통해서 더욱 쉽게 공부해보시고 문제를 풀어보세요.

1. [SQL] 서브쿼리는 언제 쓰는걸까? 
   https://project-notwork.tistory.com/38

2. [SQLD] 서브 쿼리 (SubQeury) 개념 및 종류
   https://bommbom.tistory.com/entry/%EC%84%9C%EB%B8%8C-%EC%BF%BC%EB%A6%ACSub-Query-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%A2%85%EB%A5%98


### 🎉 수고하셨습니다.
