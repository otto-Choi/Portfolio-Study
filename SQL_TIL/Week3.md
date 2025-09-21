# SQL_BASIC 3주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_3rd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**3주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **7 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 3문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_3rd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-6. 연습문제 1~3번

### 2-6. 연습문제 7~9번

### 2-6. 연습문제 10~12번

### 2-6. 연습문제 13~17번

### 2-7. 정리 

### 2-8. 새로운 집계함수



## 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-1. INTRO

### 3-2. SQL 쿼리 작성하는 흐름

### 3-3. 쿼리 작성 템플릿과 생산성 도구 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 2-6. 연습문제

~~~
✅ 학습 목표 :
* 연습문제(7문제 이상) 푼 것들 정리하기
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

#### 1번. 포켓몬 중에 type2가 없는 포켓몬의 수를 작성하는 쿼리를 작성해주세요.

답안
~~~sql
select
  count(id) as count
from basic.pokemon
where
  type2 is null
~~~

모범답안
~~~sql
SELECT
    COUNT(id) AS CNT
FROM basic.pokemon
WHERE
    type2 IS NULL
~~~
<hr/>

#### 2번. type2가 없는 포켓몬의 type1과 type1의 포켓몬 수를 알려주는 쿼리를 작성해 주세요. 단, type1의 포켓몬 수가 큰 순으로 정렬해주세요.

답안
~~~sql
select
  type1,
  COUNT(id) AS CNT
from basic.pokemon
where
  type2 is null
group by type1
~~~
모범답안
~~~sql
SELECT
    type1,
    COUNT(id) AS pokemon_cnt
FROM basic.pokemon
WHERE
    type2 IS NULL
GROUP BY
    type1
ORDER BY
    pokemon_cnt DESC
~~~
내림차순 하는 것을 누락했다. 단순 실수이긴 하지만 제시된 모든 요구사항을 빠트리지 않는 것은 매우 중요하다.
<hr/>

#### 3번. type2 상관없이 type1의 포켓몬 수를 알 수 있는 쿼리를 작성해주세요.
답안
~~~sql
select
  type1,
  COUNT(id) AS CNT
from basic.pokemon
group by type1
~~~
모범답안
~~~sql
SELECT
    type1,
    COUNT(id) AS pokemon_cnt
FROM basic.pokemon
GROUP BY
    type1
~~~
<hr/>

#### 4번. 전설 여부에 따른 포켓몬 수를 알 수 있는 쿼리를 작성해주세요.
답안
~~~sql
select
  is_legendary,
  count(id) as cnt
from basic.pokemon
group by is_legendary
~~~
모범답안
~~~sql
SELECT
    is_legendary,
    COUNT(id) AS pokemon_cnt
FROM basic.pokemon
GROUP BY
    is_legendary
~~~
<hr/>

#### 5번. 동명이인이 있는 이름은 무엇일까요?
답안
~~~sql
select
  name,
  count(name) as cnt
from basic.trainer
group by name
having
  cnt > 1
~~~
모범답안
~~~sql
SELECT
    name,
    COUNT(name) AS trainer_cnt
FROM basic.trainer
GROUP BY
    name
HAVING
    trainer_cnt >= 2
~~~
<hr/>

#### 6번. trainer 테이블에서 "Iris" 트레이너의 정보를 알 수 있는 쿼리를 작성해주세요.
답안
~~~sql
select
  *
from basic.trainer
where
  name = 'Iris'
~~~
모범답안
~~~sql
SELECT
    *
FROM basic.trainer
WHERE
    name = "Iris"
~~~
<hr/>

#### 7번. trainer 테이블에서 "Iris", "Whitney", "Cynthia" 트레이너의 정보를 알 수 있는 쿼리를 작성해주세요.
답안
~~~sql
select
  *
from basic.trainer
where
  name in ("Iris", "Whitney", "Cynthia")
~~~
모범답안
~~~sql
SELECT
    *
FROM basic.trainer
WHERE
    name IN ("Iris", "Whitney", "Cynthia")
~~~
<hr/>

#### 8번. 전체 포켓몬 수는 얼마나 되나요?
답안
~~~sql
select
  count(id)
FROM basic.pokemon
~~~
모범답안
~~~sql
SELECT
    COUNT(ID) AS pokemon_cnt
FROM basic.pokemon
~~~
<hr/>

#### 9번. 세대별로 포켓몬 수가 얼마나 되는지 알 수 있는 쿼리를 작성해주세요.
답안
~~~sql
SELECT
  generation,
  count(id)
FROM basic.pokemon
GROUP BY
  generation
~~~
모범답안
~~~sql
SELECT
   generation,
  count(id) AS pokemon_cnt
FROM basic.pokemon
GROUP BY
  generation
~~~
이 때 부터 답안의 구문들을 대문자로 적었다. 아무래도 가독성을 위해 대문자를 주로 사용하는 것이 좋을 것 같다.
<hr/>

#### 10번. type2가 존재하는 포켓몬의 수는 얼마나 되나요?
답안
~~~sql
SELECT
  count(type2) as cnt
FROM basic.pokemon
~~~
모범답안
~~~sql
SELECT
  count(type2) as pokemon_cnt
FROM basic.pokemon
WHERE
  type2 IS NOT NULL
~~~
결과적으론 같은 값이 나오나, WHERE을 철저히 하는게 오류를 줄일 수 있을 것 같다.
<hr/>

#### 11번. type2가 있는 포켓몬 중에 제일 많은 type1은 무엇인가요?
답안
~~~sql
SELECT
  MAX(type1)
FROM basic.pokemon
WHERE
  type2 IS NOT NULL
~~~
모범답안
~~~sql
SELECT
  type1,
  COUNT(id) AS pokemon_cnt
FROM basic.pokemon
WHERE
  type2 IS NOT NULL
GROUP BY
  type1
ORDER BY
  pokemon_cnt
LIMIT 1
~~~
우선, 내가 낸 답은 질문에 대한 정답은 맞췄다. 그러나 세부적인 정보를 제공하기도 하고 활용하는 측면에서 모범답안이 더 낫다.

또한, LIMIT 문에 대해 새로 알게 되었다. 이는 결과물의 ROW 수를 제한하는 기능을 수행한다.
<hr/>

#### 12번. 단일(하나의 타입만 있는) 타입 포켓몬 중 가장 많은 type1은 무엇일까요?
답안
~~~sql
SELECT
  MAX(type1)
FROM basic.pokemon
WHERE
  type2 IS NULL
~~~
모범답안
~~~sql
SELECT
  type1,
  COUNT(id) AS pokemon_cnt
FROM basic.pokemon
WHERE
  type2 IS NULL
GROUP BY
  type1
ORDER BY
  pokemon_cnt
LIMIT 1
~~~
이 문제도 마찬가지다. 추가적인 정보를 제공하는 답안이다.
<hr/>

#### 13번. 포켓몬의 이름에 "파"가 들어가는 포켓몬은 어떤 포켓몬이 있을까요?
답안
~~~sql
SELECT
  *
FROM basic.pokemon
WHERE
  kor_name LIKE "%파%"
~~~
모범답안
~~~sql
SELECT
  kor_name
FROM basic.pokemon
WHERE
  kor_name LIKE "%파%"
~~~
이름만 출력하는 모범답안이다.

추기적으로, LIKE문에 대해 새로 알게 되었다. 문자열 컬럼에서 특정 단어가 포함되어 있는지 알고 싶은 경우에 LIKE를 사용하면 편리하다. 또한 %를 이용하면 더 편리하다.
<hr/>

#### 14번. 뱃지가 6개 이상인 트레이너는 몇명이 있나요?
답안
~~~sql
SELECT
  count(id) as cnt
FROM basic.trainer
WHERE
  badge_count >= 6
~~~
모범답안
~~~sql
SELECT
  count(id) as trainer_cnt
FROM basic.trainer
WHERE
  badge_count >= 6
~~~
<hr/>

#### 15번. 트레이너가 보유한 포켓몬이 가장 많은 트레이너는 누구일까요?
답안
~~~sql
SELECT
  trn.name
FROM basic.trainer as trn
WHERE trn.id = (
SELECT
  tp.trainer_id,
FROM basic.trainer_pokemon as tp
GROUP BY tp.trainer_id
ORDER BY COUNT(tp.pokemon_id) DESC
LIMIT 1
)
~~~
모범답안
~~~sql
SELECT
  trainer_id,
  COUNT(pokemon_id) AS pokemon_cnt
FROM basic.trainer_pokemon
GROUP BY
  trainer_id
~~~
누구인지 물어봐서 이름을 구하고자 했고, 이는 trainer 테이블에 존재하여 서브쿼리를 활용했다. 중간에 시행착오를 거쳤는데, 이는 서브쿼리에서 출력하는 값이 row 형태여서 생기는 오류였다.
<hr/>

#### 16번. 포켓몬을 가장 많이 풀어준 트레이너는 누구일까요?
답안
~~~sql
SELECT
  trn.name
FROM basic.trainer as trn
WHERE trn.id = (
SELECT
  tp.trainer_id,
FROM basic.trainer_pokemon as tp
WHERE tp.status = "Released"
GROUP BY tp.trainer_id
ORDER BY COUNT(tp.pokemon_id) DESC
LIMIT 1
)
~~~
모범답안
~~~sql
SELECT
  trainer_id,
  COUNT(pokemon_id) AS pokemon_cnt
FROM basic.trainer_pokemon
WHERE
  status = "Released"
GROUP BY
  trainer_id
ORDER BY
  pokemon_cnt DESC
LIMIT 1
~~~
마찬가지로 이름을 출력하도록 서브쿼리를 활용했다.
<hr/>

#### 17번. 트레이너 별로 풀어준 포켓몬의 비율이 20%가 넘는 포켓몬 트레이너는 누구일까요? (풀어준 포켓몬의 비율 = 풀어준 포켓몬의 수 / 전체 포켓몬의 수)
답안
~~~sql
SELECT
  trn.name
FROM 
basic.trainer as trn
JOIN (
SELECT
  trainer_id,
  COUNTIF(status = "Released") as rlsd,
  COUNTIF(status != "Released") as nrlsd
FROM basic.trainer_pokemon
GROUP BY
  trainer_id
HAVING
  rlsd / (nrlsd+rlsd) >= 0.2
) AS tp
ON trn.id = tp.trainer_id
~~~
모범답안
~~~sql
SELECT
  trainer_id,
  COUNTIF(status = "Released") AS released_cnt,
  COUNT(pokemon_id) AS pokemon_cnt,
  COUNTIF(status = "Released")/COUNT(pokemon_id) AS released_ratio
FROM basic.trainer_pokemon
GROUP BY
  trainer_id
HAVING
  released_ratio >= 0.2
~~~
countif를 두번 쓸 필요가 없었다. 그 외에는 추가적인 수행을 위해 조인을 활용하기도 했다.

## 2-8. 새로운 집계함수

~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE을 활용하는 방법을 설명할 수 있다. 
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

- 기존 집계함수
  - AVG
  - COUNT
  - COUNTIF
  - SUM
  - MAX
  - MIN
- 새로운 집계함수
  - GROUP BY ALL : 기존에 컬럼을 모두 명시해야 했던 것과 달리 훨씬 유용하다.

## 3-2. 쿼리를 작성하는 흐름

~~~
✅ 학습 목표 :
* 쿼리를 작성하는 흐름을 설명할 수 있다.
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

1. 지표 고민 : 어떤 문제를 해결하기 위해 데이터가 필요한가?
2. 지표 구체화 : 추상적이지 않고 구체적인 지표 명시(분자, 분모 표시)
3. 지표 탐색 : 유사한 문제를 해결한 케이스가 있나 확인하고, 존재한다면 해당 쿼리 리뷰
4. 쿼리 작성 : 데이터가 있는 테이블 찾기, 만약 2개 이상의 테이블에 있다면 연결 방법 고민
5. 데이터 정합성 확인 : 예상한 결과와 동일한지 확인
6. 쿼리 가독성 : 나중을 위해 깔끔하게 쿼리 작성
7. 쿼리 저장 : 쿼리는 재사용되므로 문서로 저장

## 3-3. 쿼리 작성 템플릿과 생산성 도구

~~~
✅ 학습 목표 :
* 생산성 도구를 만들 수 있다.
~~~

<!-- 이어질 주차에서 생산성 도구를 활용한 실습이 있습니다.강의에 맞게 제작하여 화면을 캡쳐하여 이 주석을 지우고 올려주세요. -->

쿼리 작성 템플릿
```
# 쿼리를 작성하는 목표, 확인할 지표 : 
# 쿼리 계산 방법 : 
# 데이터의 기간 : 
# 사용할 테이블 : 
# Join KEY : 
# 데이터 특징 : 
```
~~~sql
SELECT

FROM
WHERE
~~~


<br>
<br>

---

# 2️⃣ 학습 인증란

<!-- 이 글을 지우고, 여기에 학습한 것을 인증해주세요.-->

<img width="396" height="883" alt="image" src="https://github.com/user-attachments/assets/77df7f14-b148-4b44-a0ea-91931f5848ff" />


<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. 포켓몬 게임에 재미를 느낀 동혁은 포켓몬 도감에서 강력한 포켓몬 타입을 미리 선점하기 위해, 먼저 어떤 포켓몬들이 있는지 포켓몬 수를 기준으로 내림차순 정렬하여  확인하고자 했습니다.**
>
> 그래서 다음과 같은 필요한 정보를 미리 정리해보았습니다. 

~~~
조건 : type2는 상관없이
보고 싶은 컬럼 : type1
집계 내용 : 각 type1 별 포켓몬 수
정렬 기준 : 포켓몬 수를 기준으로 내림차순 정렬
~~~

> **이 목표를 바탕으로 동혁이 아래와 같은 쿼리를 잘 작성했지만, 일부 SQL 문법 요소를 빼먹었습니다. 비어 있는 부분인 ㄱ,ㄴ,ㄷ 에 들어갈 알맞은 SQL 구문을 채워보세요:**

~~~sql
SELECT type1, (ㄱ)
FROM pokemon
(ㄴ) type1
ORDER BY (ㄱ) (ㄷ);
~~~



~~~
여기에 답을 작성해주세요!
~~~



### 🎉 수고하셨습니다.
