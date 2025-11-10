# SQL_BASIC 6주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_6th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**6주차 과제는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_6th

### 섹션 6. 다량의 자료를 연결 : JOIN 

### 5-1. Intro

### 5-2. JOIN 이해하기

### 5-3. 다양한 JOIN 방법

### 5-4. JOIN 쿼리 작성하기 

### 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분

### 5-6. JOIN 연습문제 1~2번

### 5-6. JOIN 연습문제 3~5번

### 5-7. 정리



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | ✅         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<!-- 여기까진 그대로 둬 주세요-->

<br>

---

# 1️⃣ 개념정리

## 5-2. JOIN 이해하기

~~~
✅ 학습 목표 :
* JOIN에 대한 정의와 필요성에 대해 설명할 수 있다.
~~~

JOIN: 서로 다른 데이터 테이블을 연결하는 것

JOIN의 필요성
- 관계형 데이터베이스(RDBMS) 설계시 정규화 과정을 거침
    - 정규화는 중복을 최소화하게 데이터를 구조화
    - User Table은 유저 데이터만, Order Table은 주문 데이터만
    - 따라서 데이터를 다양한 Table에 저장해서 필요할 때 JOIN 사용
- 데이터는 개발 관점에서 분리되어 있는 것이 낫다.


<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->



## 5-3. 다양한 JOIN 방법

~~~
✅ 학습 목표 :
* JOIN 방법들의 종류를 설명할 수 있다. 
* 각 JOIN 방법들의 차이점에 대해서 설명할 수 있다. 
~~~

- (INNER) JOIN
    - 두 테이블의 공통 요소만 연결
- LEFT/RIGHT (OUTER) JOIN
    - 왼쪽/오른쪽 테이블을 기준으로 연결
- FULL (OUTER) JOIN
    - 양쪽 모두를 연결
- CROSS JOIN
    - 두 테이블의 각각의 요소에 대해 모든 조합



<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->



## 5-4. JOIN 쿼리 작성하기 

~~~
✅ 학습 목표 :
* JOIN을 사용한 문법에 대해 이해하여 적용할 수 있다.
* JOIN 을 활용한 쿼리를 작성할 수 있다. 
~~~

SQL JOIN 쿼리 작성 흐름

1.  테이블 확인
    - 테이블에 저장된 데이터, 컬럼 확인
2. 기준 테이블 정의
    - 가장 많이 참고할 기준 테이블 정의
3. JOIN Key 찾기
    - 여러 Table과 연결할 Key 정리
4. 결과 예상하기
    - 결과 테이블을 예상해서 손, 엑셀로 작성
5. 쿼리 작성/검증
    - 예상한 결과와 동일한 결과가 나오는지 확인

JOIN이라는 구문을 이용하는 경우, CROSS JOIN을 제외한 모든 조인에서 ON이 필수적이다.

다만, FROM 과 WHERE을 함께 이용하는 경우 ON이 필요하지 않다.



<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->



## 5-6. JOIN 연습문제 1~5번 

~~~
✅ 학습 목표 :
* 연습문제(3문제 이상) 푼 것들 정리하기
~~~

1번. 트레이너가 보유한 포켓몬들이 얼마나 있는지 알 수 있는 쿼리를 작성해 주세요.

답안
~~~sql
SELECT p.kor_name AS pokemon_name, COUNT(*) AS pokemon_cnt
FROM basic.trainer_pokemon tp
LEFT JOIN basic.pokemon p
  ON tp.pokemon_id = p.id
WHERE tp.status IN ('Active', 'Training')
GROUP BY p.kor_name;
~~~

정답과 동일한 결과를 가져온다. 다만, 문제에서 제시되지 않은 정렬을 포함할 경우 동일하다.

---

2번. 각 트레이너가 가진 포켓몬 중에서 'Grass' 타입의 포켓몬 수를 계산해주세요. (단, 편의를 위해 type1 기준으로 계산해주세요.)

답안
~~~sql
SELECT t.name, COUNT(*) cnt
FROM
  basic.trainer_pokemon tp,
  basic.pokemon p,
  basic.trainer t
WHERE tp.pokemon_id = p.id
  AND tp.trainer_id = t.id
  AND p.type1 = 'Grass'
GROUP BY t.name;
~~~

FROM과 WHERE을 이용한 조인으로 풀이해 봤다. '가진'이라는 조건이 붙었기에 Active, Traing으로 조건을 걸어야 했다.

---

3번. 트레이너의 고향(hometown)과 포켓몬을 포획한 위치(location)를 비교하여, 자신의 고향에서 포켓몬을 포획한 트레이너의 수를 계산해주세요.

답안
~~~sql
SELECT COUNT(*)
FROM basic.trainer_pokemon tp
LEFT JOIN basic.trainer t
  ON tp.trainer_id = t.id
WHERE tp.location = t.hometown;
~~~

고향과 위치가 같았던 횟수가 아닌, 트레이너의 수를 찾아야 하므로 DISCTINCT가 필요했다.

SELECT 문을 SELECT COUNT(distinct tp.trainer_id)으로 수정하여 정답을 도출했다.

---

4번. Master 등급인 트레이너들은 어떤 타입의 포켓몬을 제일 많이 보유하고 있을까요?

답안
~~~sql
SELECT p.type1
FROM basic.trainer t,
  basic.pokemon p,
  basic.trainer_pokemon tp
WHERE
  tp.trainer_id = t.id
  AND tp.pokemon_id = p.id
  AND t.achievement_level = 'Master'
  AND tp.status IN ('Active', 'Training')
GROUP BY p.type1
ORDER BY COUNT(*) DESC
LIMIT 1;
~~~

WHERE으로 조인했다.

---

5번. Incheon 출신 트레이너들은 1세대, 2세대 포켓몬을 각각 얼마나 보유하고 있나요?

답안
~~~sql
SELECT t.name,
  SUM(CASE WHEN p.generation = 1 THEN 1 ELSE 0 END) AS gen1_cnt,
  SUM(CASE WHEN p.generation = 2 THEN 1 ELSE 0 END) AS gen2_cnt
FROM basic.trainer t,
  basic.pokemon p,
  basic.trainer_pokemon tp
WHERE
  tp.trainer_id = t.id
  AND tp.pokemon_id = p.id
  AND t.hometown = 'Incheon'
  AND tp.status IN ('Active', 'Training')
  AND p.generation IN (1, 2)
GROUP BY t.name
~~~
WHERE으로 join했으며, SUM, CASE문을 이용해 각 트레이너별로 1세대, 2세대 포켓몬 보유 수를 확인했다.
정답에선 각 세대별로 총 개수를 구했다.


<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->




<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/164673

> 조건에 부합하는 중고거래 댓글 조회하기 (JOIN)

~~~sql
SELECT b.TITLE, r.BOARD_ID, r.REPLY_ID, r.WRITER_ID, r.CONTENTS, DATE_FORMAT(r.CREATED_DATE, '%Y-%m-%d') AS CREATED_DATE
FROM USED_GOODS_BOARD b, USED_GOODS_REPLY r
WHERE b.BOARD_ID = r.BOARD_ID
AND b.CREATED_DATE BETWEEN '2022-10-01' AND '2022-10-31'
ORDER BY r.CREATED_DATE, b.TITLE
~~~
JOIN을 ON이 아닌 WHERE을 통해 수행했다.<br>
FROM USED_GOODS_BOARD b<br>
LEFT JOIN USED_GOODS_REPLY r<br>
ON b.BOARD_ID = r.BOARD_ID<br>
로도 구현 가능하다.
<img width="729" height="326" alt="image" src="https://github.com/user-attachments/assets/80885d23-b3f0-4cf4-806f-76c3ea599460" />


https://school.programmers.co.kr/learn/courses/30/lessons/144854

> 조건에 맞는 도서와 저자 리스트 출력하기 (JOIN)
~~~sql
SELECT b.BOOK_ID, a.AUTHOR_NAME, DATE_FORMAT(b.PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE
FROM BOOK b
LEFT JOIN AUTHOR a
    ON b.AUTHOR_ID = a.AUTHOR_ID
WHERE b.CATEGORY = '경제'
ORDER BY b.PUBLISHED_DATE
~~~

<img width="719" height="320" alt="image" src="https://github.com/user-attachments/assets/79b09e31-715e-4e26-a97b-cb6f0366102a" />

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. --> 



---

# 3️⃣ 참고자료

JOIN 에 대해서 그림으로 쉽게 이해할 수 있는 자료들도 있어서 첨부합니다. 아래의 블로그도 학습할 때 같이 참고해주세요.

1. https://data-marketing-bk.tistory.com/entry/SQL-JOIN-%ED%95%9C-%EB%B0%A9%EC%97%90-%EC%A0%95%EB%A6%AC-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%BD%94%EB%93%9C%EA%B9%8C%EC%A7%80-%EC%9D%B4%EA%B2%83%EB%A7%8C-%EB%B3%B4%EC%9E%90



2. https://velog.io/@wijoonwu/JOIN

<br>

### 🎉 수고하셨습니다.
