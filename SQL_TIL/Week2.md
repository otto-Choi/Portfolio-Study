# SQL_BASIC 2주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_2nd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**2주차 과제**는 1주차 과제처럼 SQL의 필요성이나 느낀점 위주가 아닌, **실제 강의 내용을 바탕으로 개념을 정리하고 학습한 내용을 집중적으로 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요. 

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_2nd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

### 2-4. SELECT 연습문제

### 2-5. 집계 (Group By + Having + Sum/Count)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | 🍽️         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리 

## 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE의 핵심 문법을 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

SELECT를 사용할 때, 여러 열을 가져오고 싶은 경우 ,를 사이에 넣는다.<br>
FROM을 사용할 때 " 또는 '를 붙이지 않는다.<br>
WHERE을 사용할 때 문자형인 경우 "를 사용한다.<br>
AS 사용시 " 또는 '를 붙이지 않는다.<br>
```
SELECT: 테이블의 어떤 컬럼을 선택(출력)할 것인가?
    Col1 AS new_name: Col1을 new_name으로 가져온다
    EXCEPT Col2: Col2를 제외한 모든 컬럼
FROM Dataset.Table: 어떤 테이블에서 데이터를 확인할 것인가?
WHERE: 만약 원하는 조건이 있다면 어떤 조건인가?
```

테이블 명 기입시 대소문자를 구분한다.

## 2-5. 집계 (Group By / HAVING / SUM,COUNT)

~~~
✅ 학습 목표 :
* 데이터를 집계하고 그룹화하는 방법을 설명할 수 있다.
* GROUP BY, HAVING, ORDER BY, 집계함수(SUM/COUNT 등)을 활용하는 방법을 설명할 수 있다.
* having과 where의 차이에 대해서 설명할 수 있다.
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->
GROUP BY: 같은 값끼리 모아서 그룹화한다.<br>
이 때, 특정 컬럼을 기준으로 모으면서 다른 컬럼에선 집계 가능하다.(합, 평균, MAX, MIN 등)

그룹화 활용 예시
- 일자별 집계
- 연령대별 집계
- 특정 타입별 집계
- 앱 화면별 집계
- 기타 등등

```
SELECT<br>
    집계할_컬럼1,
    집계 함수(COUNT, MAX, MIN 등)
FROM Table
GROUP BY
    집계할_컬럼1
```

- AVG: 평균
- COUNT: 개수(ROW)
- COUNTIF: 조건부 개수
- MAX: 최대값
- MIN: 최소값
- SUM: 합계
- DISTINCT: 여러 값 중 Unique한 것만 보고 싶은 경우 사용(중복 제거)

함수 사용시 엑셀 처럼 괄호를 사용한다.<br>
예: count(id)

distinct 사용시 띄어쓰기를 사용한다.<br>
예: count(distinct id)

<hr/>


ORDER BY <컬럼> <순서><br>
- DESC: 내림차순
- OSC: 오름차순(보통 Default이므로 기재할 필요 없을 듯 하다.)
- ORDER BY는 쿼리의 맨 마지막(아래)에 두고, 쿼리의 맨 마지막에만 작성하면 됨(중간엔 필요없음)

LIMIT
- 쿼리문의 결과 Row 수를 제한하고 싶은 경우 사용
- 쿼리문의 제일 마지막에 작성

<hr/>

WHERE과 HAVING 비교
- WHERE
    - 테이블에 바로 조건을 사용하고 싶은 경우 사용
    - Raw Data인 테이블 데이터에서 조건 설정
- HAVING
    - GROUP BY 한 후 조건을 설정하고 싶은 경우 사용
    - 집계 하여 AS로 지정한 열에 대한 조건을 설정하는 경우 이를 활용한다.
    - 서브쿼리 활용시 HAVING 대신 WHERE을 쓸 수는 있다.

서브 쿼리
- SELECT 문 안에 존재하는 SELCET 쿼리
- FROM 절에 또 다른 SELECT 문을 넣을 수 있음
- 괄호로 묶어서 사용

<hr/>

요약, 집계, 그룹화 정리
|상황|사용하는 기능|
|---|---|
집계하고 싶은 경우|GROUP BY + 집계함수(AVG, MAX 등)
고유값을 알고 싶은 경우|DISTINCT
조건을 설정하고 싶은 경우|WHERE/HAVING
정렬하고 싶은 경우|ORDER BY
출력 개수를 제한하고 싶은 경우|LIMIT

<br>




# 2️⃣ 학습 인증란

<!-- 이 글을 지우고, 여기에 학습한 것을 인증해주세요.-->

<img width="402" height="210" alt="image" src="https://github.com/user-attachments/assets/cc0558fe-edf2-4152-9086-4f0863c34359" />


<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. 포켓몬 마스터 승화는 포켓몬 데이터 조회하는 SQL문에 재미를 느껴서 혼자서 데이터를 조회하는 쿼리문을 짰습니다. 하지만 세 가지의 오류로 다음 코드가 실행이 안된다고 하는데, 각 오류의 위치와 이유를 설명하고, 올바른 쿼리문으로 수정해보세요.**

~~~sql
# 승화의 SQL Query문 
SELECT name AS '포켓몬 이름', ID;
WHERE type = 'Electric';
FROM pokemon;
~~~

1. pokemon 테이블의 한국어 이름으로 지정된 열은 kor_name이다. name을 kor_name으로 수정해야 한다.
2. AS를 이용해 한국어 열을 지정하려면 백틱(`)을 활용해야 한다.
3. WHERE문은 FROM문보다 나중에 사용되어야 한다.
4. FROM문 사용시, 경로를 적절히 설정해 주어야 한다.
5. 포켓몬의 첫번째 타입에 대한 열의 이름은 type1이다. type을 type1로 수정해야 한다.
~~~
SELECT
    kor_name AS `포켓몬 이름`,
    ID
FROM basic.pokemon
WHERE
    type1 = 'Electric'
~~~



## 문제 2

> **🧚Q. 앞서 SQL Query의 오류를 해결한 승화는 기분 좋게 이번에는 포켓몬 데이터에서 타입별 평균 공격력이 60 이상인 타입만 조회하려는 쿼리를 작성하려고 했습니다. 하지만 이번에도 실수를 하여 쿼리문이 실행되지 않거나 잘못된 결과가 나오고 있는데, 쿼리에서 잘못된 부분이 무엇인지 설명하고, 올바르게 수정한 쿼리를 작성해보세요.**

~~~sql
SELECT type, AVG(attack) AS avg_attack
FROM pokemon
WHERE AVG(attack) >= 60
GROUP BY type;
~~~
1. 포켓몬의 첫번째 타입에 대한 열의 이름은 type1이다. type을 type1로 수정해야 한다.
2. FROM문 사용시, 경로를 적절히 설정해 주어야 한다.
3. 타입별 평균 공격력을 나타내는 avg_attack이 60 이상인 경우를 고르는 것이므로 GROUP BY문을 사용한다.
4. GROUP BY문을 WHERE문보다 나중에 오도록 한다.
~~~
SELECT
    type1,
    AVG(attack) AS avg_attack
FROM basic.pokemon
GROUP BY
    type1
HAVING
    avg_attack >= 60
~~~



### 🎉 수고하셨습니다.
