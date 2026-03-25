# Python 4주차 정규 과제 

📌Python 정규과제는 매주 정해진 분량의 『*파이썬 라이브러리를 활용한 데이터 분석*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **Python_4st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 참고 자료를 통해 보완하는 것이 좋습니다.

**교재 실습 예제 파일은 07_Python_Template 레포지토리의 notebooks 폴더에 업로드되어 있습니다.**

**👀(수행 인증샷은 필수입니다.)** 

## Python_4st_TIL

### 5장 판다스 시작하기 
#### 1. 판다스 자료구조 소개
#### 2. 핵심 기능
#### 3. 기술 통계 계산과 요약
#### 4. 마치며 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.25~82    | ✅         |
| 2주차 | p.83~129   | ✅         |
| 3주차 | p.131~179  | ✅         |
| 4주차 | p.181~246 | ✅         |
| 5주차 | p.247~309 | 🍽️         |
| 6주차 | p.310~379 | 🍽️         |
| 7주차 | p.381~465 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 판다스 자료구조 소개

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

- Series: 일련의 객체를 담을 수 있는 1차원 배열과 같은 자료구조

| 자료형 | 설명 |
|--------|------|
| 2차원 ndarray | 데이터를 담고 있는 행렬. 선택적으로 행과 열의 이름을 전달할 수 있다. |
| 배열, 리스트, 튜플의 딕셔너리 | 딕셔너리의 모든 항목은 길이가 동일해야 하며, 각 항목의 내용은 DataFrame의 열이 된다. |
| 넘파이의 구조화 배열 | 배열의 딕셔너리와 동일한 방식으로 취급된다. |
| Series의 딕셔너리 | Series의 각 값이 열이 된다. 명시적으로 색인을 넘겨주지 않으면 각 Series의 색인이 하나로 합쳐져서 행의 색인이 된다. |
| 딕셔너리의 딕셔너리 | 내부에 있는 딕셔너리가 열이 된다. 키의 값은 'Series의 딕셔너리'와 마찬가지로 합쳐져서 행의 색인이 된다. |
| 딕셔너리나 Series의 리스트 | 리스트의 각 항목이 DataFrame의 행이 된다. 합친 딕셔너리의 키 값이나 Series의 색인이 DataFrame의 열 이름이 된다. |
| 리스트나 튜플의 리스트 | '2차원 ndarray'의 경우와 동일한 방식으로 취급된다. |
| 다른 DataFrame | 색인을 따로 지정하지 않으면 DataFrame의 색인이 그대로 사용된다. |
| 넘파이 MaskedArray | '2차원 ndarray'의 경우와 동일한 방식으로 취급되지만 마스크 값은 반환되는 DataFrame에서 NA 값이 된다. |

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="684" height="231" alt="Image" src="https://github.com/user-attachments/assets/2caf2e87-b187-49a2-ae08-7c42f7d56b16" />

<img width="625" height="406" alt="Image" src="https://github.com/user-attachments/assets/69f00eae-4684-4f36-90f9-1ec8623e8939" />

<img width="481" height="345" alt="Image" src="https://github.com/user-attachments/assets/0bf54121-3ceb-4df2-853b-5bda7a0e3cd9" />

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->


## 2. 핵심 기능

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

| 인수 | 설명 |
|------|------|
| labels | 색인으로 사용할 새로운 순서. Index 인스턴스나 다른 순차적인 파이썬 자료구조를 사용할 수 있다. Index는 복사가 이루어지지 않고 그대로 사용된다. |
| index | 전달된 시퀀스를 새로운 행(index) 레이블로 지정한다. |
| columns | 전달된 시퀀스를 새로운 열 레이블로 지정한다. |
| axis | 색인으로 사용할 축(행 또는 열)을 지정한다. 기본값은 행(index)이다. reindex(index=new_labels) 또는 reindex(columns=new_labels)와 같이 사용할 수 있다. |
| method | 채움 메서드. ffill은 직전 값을 채워 넣고 bfill은 다음 값을 채워 넣는다. |
| fill_value | 재색인 과정 중에 새롭게 나타나는 비어 있는 데이터를 채우기 위한 값. 빈 곳의 결과에 null을 채워 넣으려면 fill_value="missing"을 이용한다. |
| limit | 전/후 보간 시에 사용할 최대 갭 크기(채워 넣을 원소의 수) |
| tolerance | 전/후 보간 시에 사용할 최대 갭 크기(값의 차이) |
| level | MultiIndex의 단계(level)에 단순 색인을 맞춘다. 그렇지 않으면 MultiIndex의 하위집합에 맞춘다. |
| copy | True인 경우 새로운 색인이 이전 색인과 동일하더라도 기본 데이터를 복사한다. False인 경우 새로운 색인이 이전 색인과 동일할 경우 복사하지 않는다. |

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="570" height="353" alt="Image" src="https://github.com/user-attachments/assets/f80d5919-8030-469f-8689-df6913eab832" />

<img width="710" height="247" alt="Image" src="https://github.com/user-attachments/assets/c64f096c-d57e-4591-aa8e-dcd15d8de8d2" />

## 3. 기술 통계 계산과 요약

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

| 메서드 | 설명 |
|--------|------|
| isin | Series의 각 원소가 넘겨받고 있는 연속된 값에 속하는지 나타내는 불리언 배열을 반환한다. |
| get_indexer | 각 값에 대해 유일한 값을 담고 있는 배열에서의 정수 색인을 계산한다. 데이터 정렬이나 조인 형태의 연산을 하는 경우에 유용하다. |
| unique | Series에서 중복되는 값을 제거하고 유일한 값만 포함하는 배열을 반환한다. 결과는 Series에서 발견된 순서대로 반환된다. |
| value_counts | Series에서 유일한 값에 대한 색인과 도수를 계산한다. 도수는 내림차순으로 정렬된다. |

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="462" height="156" alt="Image" src="https://github.com/user-attachments/assets/aecd448e-31f8-4bd1-a0ea-f078d4008e99" />

<img width="799" height="250" alt="Image" src="https://github.com/user-attachments/assets/bc56c7a8-ca03-41ba-8f1a-82b1899f9ce7" />

# 2️⃣ 실습 과제

각 문제에 대한 실행 결과가 확인되도록 코드를 작성하고 실행한 뒤, **모든 문제의 실행 화면을 캡처하여 제출하시기 바랍니다.**

**1. 아래 코드를 실행하여 도서 매출 데이터를 생성합니다.**
```python
import pandas as pd
import numpy as np

# 도서 데이터 생성
data = {
    "book_name": ["Python 기초", "Pandas 실무", "데이터 분석", "머신러닝 입문", "딥러닝 가이드"],
    "price": [25000, 32000, 28000, 45000, 38000],
    "sales_count": [15, 8, 20, 5, 12],
    "stock": [10, 5, 0, 15, 7]
}
df = pd.DataFrame(data)
```

**2. 문제**
```
1. 데이터 확인 및 기초 통계 출력
  - 문제 설명: 요약 정보 확인 
  - describe() 메서드를 사용하여 수치형 데이터(가격, 판매량 등)의 기술 통계 정보를 출력하세요.
  - print()를 이용해 기술 통계 결과를 화면에 출력하세요.

2. 특정 조건의 데이터 필터링 (불리언 색인)
  - 문제 설명: 현재 재고가 하나도 없는(0인) 도서 찾기 
  - stock 열의 값이 0인 행만 추출하여 새로운 변수에 저장하세요.
  - print()를 이용해 재고가 0인 도서의 정보를 출력하세요.

3. 새로운 열 추가 (파생 변수 생성)
  - 문제 설명: 각 도서별 총 매출액(total_revenue) 계산
  - price와 sales_count를 곱하여 total_revenue라는 새로운 열을 추가하세요.
  - print()를 이용해 새로운 열이 추가된 DataFrame의 상단 5행(head)을 출력하세요.
```

<!-- 이 부분을 지우고 인증 사진을 제출해주세요.-->
'''python
data = pd.DataFrame(data)
print(data.describe())
print()

out_of_stock = data[data["stock"] == 0]
print(out_of_stock)
print()

data["total_revenue"] = data["price"] * data["sales_count"]
print(data.head())
'''

<img width="500" height="386" alt="Image" src="https://github.com/user-attachments/assets/3a686c77-75b1-4a0f-b85c-05c13b651927" />

### 🎉 수고하셨습니다.







