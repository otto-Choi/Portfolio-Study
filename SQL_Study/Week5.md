# SQL_ADVANCED 5주차 정규 과제 

## Week 5 : 계층형 질의 & 셀프 조인

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 문제 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_5th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_5th

### 15.2.20 WITH (Common Table Expressions)

- **재귀 CTE를 통한 계층형 구조 탐색 방법을 중심으로 학습해주세요.**

> Self Join은 따로 MySQL 공식문서가 없습니다. 다른 블로그나 유튜브 영상을 참고하여 스스로 학습하고, 넣어주세요. 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 1주차 | 서브쿼리 & CTE          | ✅         |
| 2주차 | 집합 연산자 & 그룹 함수 | ✅         |
| 3주차 | 윈도우 함수             | ✅         |
| 4주차 | Top N 쿼리              | ✅         |
| 5주차 | 계층형 질의와 셀프 조인 | ✅         |
| 6주차 | PIVOT / UNPIVOT         | 🍽️         |
| 7주차 | 정규 표현식             | 🍽️         |



### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**



# 1️⃣ 학습 내용

> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 15.2.20 WITH (Common Table Expressions) : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/with.html
>
> (한국어 버전) https://dart-b-official.github.io/posts/mysql-RecursiveWith/



<br>

---

# 2️⃣ 학습 내용 정리하기

## 1. 계층형 질의 (WITH RECURSIVE)

~~~
✅ 학습 목표 :
* 'WITH RECURSIVE' 문법을 활용해 계층형 구조를 탐색할 수 있다.
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

CTE: Common Table Expreesions
- FROM 절에서 사용하는 서브쿼리를 위로 빼서 이름을 붙여놓고, 그 이름을 SELECT문에서 테이블처럼 사용하는 방법
- 선언은 WITH로 시작
- 서브쿼리에 이름을 붙여 한번 정의하고 이름으로 활용할 수 있다는 장점
- 즉, 재사용이 가능하다.
---
재귀 공통 테이블 표현식(Recursive CTE): 자신의 이름을 참조하는 서브쿼리를 가진 CTE

- CTE식은 일단 WITH절로 시작해야 한다.
- 자기참조를 이용하는 경우 RECURSIVE를 함께 적어야 한다.
- 비재귀 SELECT에선 CTE 이름을 참조하지 않는다.
- 재귀 SELECT에선 CTE 이름을 FROM을 통해 참조한다.
- 이후 재귀 SELECT에서 더 이상 새 행을 만들지 못하면 종료된다.

---
재귀 SELECT 내부에서 사용할 수 없는 사항
- 집계 함수
- 윈도 함수
- GROUP BY
- ORDER BY
- DISTINCT
---
재귀 제한과 안전장치
- cte_max_recursion_depth 
- max_execution_time




## 2. 셀프 조인

~~~
✅ 학습 목표 :
* 같은 테이블 내에서 상호 관계를 탐색할 수 있는 셀프 조인의 구조를 이해하고 사용할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

같은 테이블 내의 키를 마치 외래키처럼 참조하는 경우 이를 활용하기 위해 셀프 조인을 할 수 있다.

<br>

<br>

---

# 3️⃣ 실습 문제

## 문제 

- https://leetcode.com/problems/employees-earning-more-than-their-managers/ 

> LeetCode 181. Employees  Earning More Than Their Managers
>
> 학습 포인트 : 동일 테이블을 두 번 조인 (왜 동일 테이블을 JOIN 해야하는 문제일까)

~~~sql
SELECT e.name as Employee
FROM Employee AS e
LEFT JOIN Employee m
    ON e.managerId = m.id
WHERE e. salary > m.salary
~~~
셀프조인을 통해 간단히 작성할 수 있었다.



- https://leetcode.com/problems/tree-node/description/

> LeetCode 608. Tree Node 
>
> 학습 포인트 : id, parent_id 기반의 트리 구조에서 **부모 ~ 자식 관계 재귀 탐색**
>
> Hint : (문제 해석) 
>
> - 어떤 노드가 Root Node 이려면, 부모노드가 존재하지 않아야 한다. 
> - 어떤 노드가 Inner Node 이려면, 나를 부모로 가지는 노드가 하나 이상 존재하여야 한다.
>   - 그 외네는 모두 Leaf Node 이다. --> (CASE 문을 사용하는 것을 추천드립니다.)

~~~sql
WITH RECURSIVE CTE AS (
 SELECT
    t.id,
    t.p_id,
    0 AS depth
 FROM Tree t
 WHERE t.p_id IS NULL

 UNION ALL
 
 SELECT 
    p.id,
    p.p_id,
    c.depth + 1 AS depth
 FROM Tree p
 JOIN CTE c
    ON p.p_id = c.id
)
SELECT
    CTE.id,
    A.type
FROM CTE
LEFT JOIN(
    SELECT id,
    CASE
        WHEN b.depth = 0 THEN 'Root'
        WHEN EXISTS (SELECT 1 FROM Tree ch WHERE ch.p_id = b.id) THEN 'Inner'
        ELSE 'Leaf'
    END AS type
    FROM CTE b
) AS A
ON CTE.id = A.id;
~~~
다른 부분은 모두 작성 가능했는데, 
WHEN EXISTS (SELECT 1 FROM Tree ch WHERE ch.p_id = b.id) THEN 'Inner'
이부분은 작성에 어려움을 겪었다.



- https://school.programmers.co.kr/learn/courses/30/lessons/144856

> 프로그래머스 : 저자 별 카테고리 별 매출액 집계하기 
>
> 학습 포인트 : 카테고리와 서브카테고리 계층 구조를 분석하는 로직, SELF JOIN / CTE를 다 활용할 수 있다.
>
> - 위에 2가지의 문제를 풀어보고 난 이후, 더 편리한 방법으로 문제를 풀어보세요.
~~~sql
SELECT
    sq.AUTHOR_ID,
    sq.AUTHOR_NAME,
    sq.CATEGORY,
    SUM(sq.t) AS TOTAL_SALES
FROM (
    SELECT 
        b.AUTHOR_ID,
        A.AUTHOR_NAME,
        b.CATEGORY,
        s.SALES * b.PRICE AS t
    FROM BOOK_SALES AS s
    LEFT JOIN BOOK AS b
        ON s.BOOK_ID = b.BOOK_ID
    LEFT JOIN AUTHOR AS a
        ON b.AUTHOR_ID = a.AUTHOR_ID   
    WHERE s.SALES_DATE >= DATE '2022-01-01'
        AND s.SALES_DATE <  DATE '2022-02-01'
) as sq
GROUP BY sq.AUTHOR_ID, sq.CATEGORY
ORDER BY sq.AUTHOR_ID, sq.CATEGORY DESC
~~~
서브쿼리를 활용했다.

---

## 문제 인증란

<!-- 이 주석을 지우고 여기에 문제 푼 인증사진을 올려주세요. -->



---

# 확인문제

## 문제 1

> **🧚윤서는 어떤 기업의 조직 구조를 분석하는 SQL 쿼리를 작성하고 있습니다. 각 직원은 상위 관리자 ID(manager_id)를 가지며, 조직도는 같은 Employees 테이블 내에서 계층적으로 연결됩니다. 윤서는 최상위 관리자부터 각 사원까지의 계층 깊이(depth)를 계산하고자 다음과 같은 SELF JOIN 기반 쿼리를 시도했습니다.** 

~~~sql
SELECT e1.id, e1.name, e2.name AS manager_name
FROM Employees e1
LEFT JOIN Employees e2 ON e1.manager_id = e2.id;
~~~

> **쿼리를 잘 작성했다고 생각을 했지만, 막상 실행을 해보니 1단계 매니저까지만 추적할 수 있어 계층 구조의 전체를  표현하는데 한계가 존재했습니다. 이에 여러분에게 다음과 같은 미션을 요청합니다. WITH RECURSIVE를 활용하여  최상위 관리자부터 시작해 각 직원까지의 조직 구조 계층 깊이(depth)를 구하고, 결과를 depth가 높은 순으로 정렬하는 쿼리를 작성하세요.**



~~~
WITH RECURSIVE CTE AS (
    SELECT  
        e.id,
        e.name,
        e.manager_id,
        0 AS depth
    FROM Employees AS e
    WHERE e.manager_id IS NULL

    UNION ALL

    SELECT
        f.id,
        f.name,
        f.manager_id
        c.depth + 1 AS depth
    FROM Employees AS f
    JOIN CTE AS c
        ON f.manager_id=c.id
)
SELECT
    id,
    name,
    manager_id,
    depth
FROM org
ORDER BY depth DESC, id;
~~~



---

### 참고자료

<!--셀프조인에 대해 학습하시기에 도움이 되도록 참고할말한 잘 설명된 블로그들을 같이 첨부하겠습니다. -->

https://step-by-step-digital.tistory.com/101



<br>

### 🎉 수고하셨습니다.
