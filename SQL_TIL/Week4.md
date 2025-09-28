# SQL_BASIC 4주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_4th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**4주차 과제부터는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_4th

### 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-4. 오류를 잘 디버깅하는 방법



## 섹션 5. 데이터 탐색 - 변환

### 4-1. INTRO

### 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

### 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

### 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 3-4. 오류를 디버깅하는 방법

~~~
✅ 학습 목표 :
* 오류의 정의에 대해 설명할 수 있다. 
* 오류 메시지를 보고 디버깅이라는 과정을 수행할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->


Syntax error: 문법 오류
- Number of arguments does not match for aggregate function
  - 집계 함수의 인자 수가 일치하지 않는 경우
- SELECT list expression references column type1 a which is neither grouped nor aggregated
  - SELECT 목록 식은 다음에서 그룹화되거나 집계되지 않은 열을 참조합니다.
  - GROUP BY에 적절한 컬럼을 명시하지 않았을 경우 발생
- Expected end of input but got keyword SELECT
  - 입력이 끝날 것으로 예상되었지만 SELECT 키워드가 입력되었습니다.
- Expected end of input but got keyword WHERE at [n:n]
  - 입력이 끝날 것으로 예상되었지만 키워드 WHERE을 얻었습니다.
  - 중간에 LIMIT이 쓰여있는 경우 등 발생
- Expected ")" but got end of script at [n:n]
  - 괄호를 작성하지 않은 경우

<hr/>

오류 발견시
- 오류 메세지를 해석
- 생성형 AI, 지인 등의 도움을 받음

## 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

~~~
✅ 학습 목표 :
* 데이터 타입의 종류를 설명할 수 있다. 
* 데이터 타입을 변환하는 방법을 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

데이터 타입
- 숫자
- 문자
- 시간, 날짜
- 부울(Bool)

<hr/>

CAST문: 자료 타입을 변경하는 함수

~~~sql
SELECT
  CAST(1 AS STRING)
또는
SELECT
  SAFE_CAST(1 AS STRING) 
#자료 타입 변경이 실패할 경우 NULL 반환
~~~
SAFE_DIVIDE 등 수학 함수를 활용하면 오류를 줄일 수 있다.


## 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

~~~
✅ 학습 목표 :
* 문자열 함수들의 종류를 이해하고 어떠한 상황에서 사용하는지 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

문자열 함수
- CONCAT: 문자열 붙이기
  - CONCAT 인자로 STRING이나 숫자를 넣을 때는 데이터를 직접 넣는 것이다.
  - FROM이 없어도 실행 가능하다.
- SPLIT: 문자열 분리하기
- REPLACE: 특정 단어 수정하기
- TRIM: 문자열 자르기
- UPPER: 영어 대문자 변환
- LOWER: 영어 소문자 변환



## 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터 타입과 UTC의 개념을 설명할 수 있다. 
* DATE, DATETIME, TIMESTAMP 에 대해서 설명할 수 있다.
* 시간함수들의 종류와 시간의 차이를 추출하는 방법을 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

날짜 및 시간 데이터의 핵심
1. 날짜 및 시간 데이터 타입 파악하기: DATE, DATETIME, TIMESTAMP
2. 날짜 및 시간 데이터 관련 알면 좋은 내용: UTC, Millisecond
3. 날짜 및 시간 데이터 타입 변환하기
4. 시간 함수(두 시간의 차이, 특정 부분 추출하기)

<hr/>

- DATE: DATE만 표시하는 데이터(ex: 2025-09-24)
- DATETIME: DATE와 TIME까지 표시하는 데이터(Timezone 정보 없음): 2025-09-24T23:41:00
- TIME: 날짜와 무관하게 TIME만 표시하는 데이터: 23:41:00.00
- TIMESTAMP: UTC로부터 경과한 시간을 나타내는 값(Timezone 정보 있음): 2025-09-24 23:41:00 UTC

<hr/>

- GMT: Greenwich Mean Time(한국시간: GMT+9)
  - 영국 그리니치 천문대 기준
- UTC: Universal Time Coordinated(한국시간: UTC+9)
  - 국제적인 표준 시간
  - 협정 세계시

<hr/>

milliseconds(ms): 시간의 단위, 천분의 1초로, 빠른 반응이 필요한 분야에서 사용 <br>
microsecond(us): 1/1,000 ms





<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

<img width="397" height="517" alt="image" src="https://github.com/user-attachments/assets/780caae8-d92a-41d0-9ba6-f8967a2d51c9" />



## 프로그래머스 문제 

> 조건에 맞는 회원 수 구하기 (SELECT, COUNT) 
>
> **먼저 문제를 풀고 난 이후에 확인 문제를 확인해주세요**
>
> 문제 링크 
>
> :  https://school.programmers.co.kr/learn/courses/30/lessons/131535#

<!-- 문제를 풀기 위하여 로그인이  필요합니다. -->

<!-- 정답을 맞추게 되면, 정답입니다. 라는 칸이 생성되는데 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. --> 
<img width="593" height="264" alt="image" src="https://github.com/user-attachments/assets/4717be71-0adc-487a-bf9d-5352c4385b9b" />



```sql
SELECT
    COUNT(USER_ID)
FROM USER_INFO
WHERE
    YEAR(JOINED) = 2021 AND
    AGE >= 20 AND
    AGE <= 29
```
풀고 나서 아래 확인 문제를 보니 BETWEEN 함수를 활용하면 나이 필터링을 더 쉽게 할 수 있다는 것을 알게 되었다.


## 문제 1

> **🧚Q. 프로그래머스 문제를 풀던 서현이는 여러 번의 시행착오 끝에 결국 혼자 해결하기 어려워 오류 메시지를 공유하며 도움을 요청했습니다. 여러분들이 오류 메시지를 확인하고, 해당 SQL 쿼리에서 어떤 부분이 잘못되었는지 오류 메시지를 해석하고 찾아 설명해주세요.**

~~~sql
# 조건에 맞는 회원 수 구하기 (SELECT, COUNT) 
# 서현이의 SQL 첫 번째 풀이
SELECT COUNT(AGE, JOINED)
FROM USER_INFO
WHERE AGE BETWEEN 20 AND 29
  AND JOINED BETWEEN '2021-01-01' AND '2021-12-31';
  
오류 메시지 : Error: Number of arguments does not match for aggregate function COUNT
 
# 수정하고 난 이후 두 번째 풀이
SELECT AGE, COUNT(*)
FROM USER_INFO
WHERE AGE BETWEEN 20 AND 29
  AND JOINED BETWEEN '2021-01-01' AND '2021-12-31';
  
오류 메시지 : SELECT list expression references column AGE which is neither grouped nor aggregated
~~~

첫번째 풀이에선, COUNT 함수가 요구하는 것과 다른 개수의 요소가 포함되었다는 내용이다.<br>
SELCET COUNT(AGE, JOINED)로 1개가 아닌 2개의 컬럼이 들어가있다. COUNT(USER_ID) 또는 COUNT(*)를 이용하면 오류가 나오지 않을 것이다.

<hr/>
두번째 풀이에선, SELECT 문에서 AGE 컬럼에 대한 그룹화가 이루어지지 않았다는 오류 메세지이다.<br>
쿼리 하단에 GROUP BY AGE를 입력해 준다면 나이별로 몇명의 회원이 있는지 출력할 수 있을 것이다.






### 🎉 수고하셨습니다.
