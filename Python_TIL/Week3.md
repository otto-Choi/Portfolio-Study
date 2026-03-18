# Python 3주차 정규 과제 

📌Python 정규과제는 매주 정해진 분량의 『*파이썬 라이브러리를 활용한 데이터 분석*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **Python_3st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 참고 자료를 통해 보완하는 것이 좋습니다.

**교재 실습 예제 파일은 07_Python_Template 레포지토리의 notebooks 폴더에 업로드되어 있습니다.**

**👀(수행 인증샷은 필수입니다.)** 

## Python_3st_TIL

### 4장 넘파이 기본: 배열과 벡터 연산
#### 1. 다차원 배열 객체 ndarray
#### 2. 난수 생성
#### 3. 유니버설 함수: 배열의 각 원소를 빠르게 처리하는 함수
#### 4. 배열을 이용한 배열 기반 프로그래밍
#### 5. 배열 데이터의 파일 입출력
#### 6. 선형대수
#### 7. 계단 오르내리기 예제
#### 8. 마치며 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.25~82    | ✅         |
| 2주차 | p.83~129   | ✅         |
| 3주차 | p.131~179  | ✅         |
| 4주차 | p.181~246 | 🍽️         |
| 5주차 | p.247~309 | 🍽️         |
| 6주차 | p.310~379 | 🍽️         |
| 7주차 | p.381~465 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 다차원 배열 객체 ndarray

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

- 벡터: 크기와 방향으로 이루어진 물리량
- 스칼라: 방향이 없는 경우

- np.array: 배열을 생성
- array.shape: 각 차원의 크기를 알려주는 튜플
- array.dtype: 배열에 저장된 자료형을 알려주는 객체
- array.ndim: 배열의 차원을 알려줌

- zeros: 0이 들어있는 배열 생성
- ones: 1이 들어있는 배열 생성
- empty: 초기화되지 않은 배열 생성

스칼라 인수가 포함된 산술 연산의 경우 배열 내의 모든 원소에 스칼라 인수가 적용됨.<br>
색인 및 슬라이싱도 가능함

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="483" height="457" alt="Image" src="https://github.com/user-attachments/assets/ff5ff1fd-e9b7-44e4-aed8-15715d88129d" />

<img width="555" height="333" alt="Image" src="https://github.com/user-attachments/assets/b8a7f33f-2c2e-4a03-a030-0a3079abdc17" />

## 2. 난수 생성

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->


파이썬 내장 random보다 np.random의 속도가 훨씬 빠름.<br>


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="408" height="180" alt="Image" src="https://github.com/user-attachments/assets/5908eb30-0c7f-4ce2-96c1-0eb9795ffe2a" />

<img width="551" height="162" alt="Image" src="https://github.com/user-attachments/assets/a562273d-8c53-4429-9e46-59d78625b224" />


## 3. 유니버설 함수: 배열의 각 원소를 빠르게 처리하는 함수

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

유니버설 함수: ndarray 안의 데이터 연소별로 연산을 수행하는 함수<br>
하나 이상의 스칼라값을 받아서 하나 이상의 스칼라 결괏값을 반환하는 벡터화된 래퍼 함수임.<br>
매개변수의 개수에 따라 단항 유니버설 함수 또는 이항 유니버설 함수 등으로 분류함.


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="529" height="237" alt="Image" src="https://github.com/user-attachments/assets/65cd0a3a-0f68-4cad-91d3-1e6f34111366" />

<img width="579" height="295" alt="Image" src="https://github.com/user-attachments/assets/0eb9b4a3-5c6d-41ff-818a-ca6e0a596de0" />

## 4. 배열을 이용한 배열 기반 프로그래밍

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

numpy와 배열을 이용하여 파이썬 내장 리스트 자료형을 이용하는 것보다 훨씬 빠르고 쉽게 여러 프로그래밍을 수행할 수 있음.

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="852" height="310" alt="Image" src="https://github.com/user-attachments/assets/de716ab2-f49f-4cf7-b331-b4cd9b49b766" />

<img width="481" height="445" alt="Image" src="https://github.com/user-attachments/assets/b57c74e7-53a4-4bed-bc23-12327ecb2df5" />

<img width="627" height="694" alt="Image" src="https://github.com/user-attachments/assets/83282f86-20ae-491c-8bc5-21e30f5fcfdb" />

## 5. 배열 데이터의 파일 입출력

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="251" height="84" alt="Image" src="https://github.com/user-attachments/assets/56ba082f-1c59-42c3-a53d-95a7afd9281a" />

<img width="298" height="94" alt="Image" src="https://github.com/user-attachments/assets/6c527678-79ec-405b-a40d-38b7b7b6f9e5" />

## 6. 선형대수

### 개념정리

문돌이에게 너무나 어려웠던 부분... 아직도 이해 거의 못함...<br>
사실상 교재 필사ㅜㅜ


<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="359" height="218" alt="Image" src="https://github.com/user-attachments/assets/68c7288d-9d4d-44ac-9cb3-c662fd2f9a1c" />

<img width="532" height="426" alt="Image" src="https://github.com/user-attachments/assets/79cea1e7-3414-4a05-8128-2ef1cdcb7082" />

## 7. 계단 오르내리기 예제

### 개념정리

<!-- 이 부분을 지우고 새롭게 배우게 된 내용을 정리해주세요. -->

단일 횟수로 시행하는 경우 시행에 따라 천차만별의 결과가 나옴.<br>
따라서 마지막 걸음 후의 위치를 확인해보고, 이를 시각화해 봤음.<br>
그 결과 정규분포와 비슷한 모습을 보임.

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->

<img width="546" height="413" alt="Image" src="https://github.com/user-attachments/assets/5c3ef998-f38d-4e5c-8fc2-3b5ec9dcf146" />

<img width="554" height="413" alt="Image" src="https://github.com/user-attachments/assets/241ed355-1f67-449f-aef0-a6f7966a9934" />

<img width="492" height="353" alt="Image" src="https://github.com/user-attachments/assets/368446a6-f5fc-494d-acd7-146843276ee1" />

<img width="861" height="547" alt="Image" src="https://github.com/user-attachments/assets/4aa840fa-1a50-4040-86ec-d42e5b8f5737" />

# 2️⃣ 실습 과제

각 문제에 대한 실행 결과가 확인되도록 코드를 작성하고 실행한 뒤, **모든 문제의 실행 화면을 캡처하여 제출하시기 바랍니다.**

**1. 아래 코드를 실행하여 5일간 3개 품목의 판매량 데이터를 생성합니다.**
```python
import numpy as np

# 5일간 3개 품목의 판매량
# 행: 월, 화, 수, 목, 금 / 열: 사과, 배, 포도
sales = np.array([
    [45, 30, 75],  # 월요일
    [50, 60, 15],  # 화요일
    [85, 20, 40],  # 수요일
    [30, 90, 55],  # 목요일
    [70, 45, 80]   # 금요일
])
```

**2. 문제**
```
1. 품목별 총 판매량 계산 및 출력
  - 문제 설명: 각 품목이 5일 동안 총 몇 개 팔렸는지 계산
  - sum() 메서드의 axis 옵션을 활용하여 품목별 합계를 구하세요.
  - print()를 이용해 품목별 총 판매량 리스트를 출력하세요.

```python
sum_sales=np.sum(sales, axis=0)
print("사과: {0}개, 배: {1}개, 포도: {2}개".format(sum_sales[0], sum_sales[1], sum_sales[2]))
```

2. 특정 기간 및 품목 추출
  - 문제 설명: 수요일부터 금요일까지(3~5행), 첫 번째와 두 번째 품목(사과, 배)의 판매량만 따로 보기 
  - 배열 슬라이싱을 사용하여 해당 데이터를 추출하세요.
  - print()를 이용해 추출된 3x2 배열을 출력하세요.

```python
sales_slice = sales.copy()
sales_slice = sales_slice[2:5, 0:2]
print(sales_slice)
```

3. 목표 미달 판매량 조정
  - 문제 설명: 하루 판매량이 40개 미만인 경우, 값을 0으로 표시하고, 40개 이상인 경우는 기존 값을 유지
  - np.where() 함수를 사용하여 40 미만은 0, 40 이상은 원래 값을 가지는 새로운 배열을 만드세요.
  - print()를 이용해 수정된 배열을 출력하세요.

```python
sales_40 = sales.copy()
sales_40 = np.where(sales_40 < 40, 0, sales_40)
print(sales_40)
```

<!-- 이 부분을 지우고 인증 사진을 제출해주세요.-->

<img width="760" height="814" alt="Image" src="https://github.com/user-attachments/assets/aa766b28-7a0a-4bc8-9d64-bfcf641e0377" />

### 🎉 수고하셨습니다.







