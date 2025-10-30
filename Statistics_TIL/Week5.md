# 통계학 5주차 정규과제

📌통계학 정규과제는 매주 정해진 분량의 『*데이터 분석가가 반드시 알아야 할 모든 것*』 을 읽고 학습하는 것입니다. 이번 주는 아래의 **Statistics_5th_TIL**에 나열된 분량을 읽고 `학습 목표`에 맞게 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 추가자료와 교재를 다시 참고하여 보완하는 것이 좋습니다.

5주차는 `2부-데이터 분석 준비하기`를 읽고 새롭게 배운 내용을 정리해주시면 됩니다


## Statistics_5th_TIL

### 2부. 데이터 분석 준비하기

### 10. 데이터 탐색과 시각화

- 10.5 분포 시각화부터 10장 마지막 10.8 박스 플롯까지 진행해주시면 됩니다.

**(수행 인증샷은 필수입니다.)** 

<!-- 이번주는 확인 문제가 없고, 교재의 실습에 있는 부분을 따라해주시면 됩니다. 데이터셋과 참고자료는 노션의 정규과제란에 있는 깃허브를 활용해주시면 됩니다. -->

## Study ScheduleStudy Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | 1부 p.2~46    | ✅         |
| 2주차 | 1부 p.47~81   | ✅         |
| 3주차 | 2부 p.82~120  | ✅         |
| 4주차 | 2부 p.121~167 | ✅         |
| 5주차 | 2부 p.168~202 | ✅         |
| 6주차 | 3부 p.203~250 | 🍽️         |
| 7주차 | 3부 p.251~299 | 🍽️         |

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념 정리 

## 10. 데이터 탐색과 시각화

```
✅ 학습 목표 :
* EDA의 목적을 설명할 수 있다.
* 주어진 데이터셋에서 이상치, 누락값, 분포 등을 식별하고 EDA 결과를 바탕으로 데이터셋의 특징을 해석할 수 있다.
* 공분산과 상관계수를 활용하여 두 변수 간의 관계를 해석할 수 있다.
* 적절한 시각화 기법을 선택하여 데이터의 특성을 효과적으로 전달할 수 있다.
```

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->

---
Week4에 시각화 파트를 끝까지 읽고 수행했던 내용을 그대로 옮겨왔습니다.
---

분포시각화

연속형과 같은 양적 척도인지, 명목형과 같은 질적 척도인지에 따라 구분해서 그린다. 양적 척도의 경우 막대그래프나 선그래프로 분포를 나타낼 수도 있고, 히스토램을 통해 분포를 단순화하여 보다 알아보기 쉽게 만들었다.

질적 척도로 이루어진 변수는 구성이 단순한 경우 파이차트나 도넛차트를 사용한다.<br>
구성요소가 복잡한 질적 척도를 표현할 때는 트리맵 차트를 이용하면 보다 효과적으로 표현할 수 있다.<br>
와플 차트는 일정한 네모난 조각들로 분포를 표현하는데, 트리맵 차트처럼 위계궂를 표현하지는 못한다.

<hr/>

관계 시각화

산점도를 그릴 때는 극단치를 제거하고서 그리는 것이 좋다. 극단치로 인해 주요 분포 구간이 압축되어 시각화의 효율이 떨어지기 때문이다.

데이터가 너무 많아서 점들이 서로 겹쳐 정보를 제대로 확인하기 어려울 때 각각의 점에 투명도를 주어 점들의 밀도를 함께 표현할 수 있도록 할 수가 있다.

버블차트를 이용하면 세 가지 요소의 상관관계를 표현할 수 있다. 크기, 색상까지 이용하면 네 가지 요소까지도 표현할 수도 있다.

<hr/>

공간 시각화

정보를 효과적으로 전달할 수 있도록 거시적에서 미시적으로 진행되는 분석 방향과 같이 스토리라인을 잡고 시각화를 적용하는 것이 좋다.

종류로는, 도트맵, 버브랩, 코로플레스맵, 커넥션맵 등이 있다.

<hr/>

박스플롯(상자 수염 그림)

네모 상자 모양에 최댓값과 최솟값을 나타내는 선이 결합된 모양의 데이터 시각화 방법<br>
하나의 그림으로 양적 척도 데이터의 분포 및 편향성, 평균과 중앙값 등 다양한 수치를 보기 쉽게 정리해 준다.

박스플롯의 다섯 가지 수치
- 최솟값: 제1사분위에서 1.5 IQR을 뺀 위치
- 제1사분위(Q1): 25%의 위치(작은 값)
- 제2사분위(Q2): 50%의 위치(중앙값을 의미)
- 제3사분위(Q3): 75%의 위치(큰 값)
- 최댓값: 제3사분위에서 1.5 IQR을 더한 위치

박스플롯을 해석할 때는 항상 데이터 분포를 함께 떠올리는 습관이 필요하다.<br>
평균값과 중앙값 간 차이가 큰 경우 정규분포와 형태의 차이가 클 것으로 판단할 수 있다.


<br>
<br>

---

# 2️⃣ 확인 과제

> **교재에 있는 실습 파트를 직접 따라 해보세요. 실습을 완료한 뒤, 결과화면(캡처 또는 코드 결과)을 첨부하여 인증해 주세요.단순 이론 암기보다, 직접 손으로 따라해보면서 실습해 보는 것이 가장 확실한 학습 방법입니다.**
>
> > **인증 예시 : 통계 프로그램 결과, 시각화 이미지 캡처 등**

<!-- 이 주석을 지우고 “실습 결과 화면(캡처)을 이곳에 첨부해주세요.-->



<img height="240" alt="image" src="https://github.com/user-attachments/assets/2e509b87-e65f-4954-8501-3f6e91b489de" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/0a27227e-becb-43ab-909e-c34ded06ecac" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/01018170-68eb-4aeb-a5bf-9a68398bc9d3" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/d47f5a50-3985-49f1-982e-ad84dee50c3f" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/3825c4cc-5d99-43f3-901d-88b4db5a963e" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/331c6775-1926-4060-9585-725384b6857a" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/9dfa5637-cdbb-47b1-a45d-c42fbbdc08fe" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/b0036d56-fc06-42dd-b17c-987e67ca900f" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/8547a0f3-ac3f-4f29-b1cc-614dde0efbb3" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/76ae8645-951d-4417-9839-19931061081c" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/e63e8569-5e32-4d0d-9250-1a832255c0ce" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/9f49b2a2-ff99-4fb9-bb91-54830b8e4ec3" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/ecbf04ea-fe2c-4a47-8137-a4f89d8f010e" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/f41da5d5-cd51-4f76-82f8-893ca4611301" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/c4d6cd0a-ef16-4efa-a254-cfda605eb68b" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/0d86ce6b-85e8-4e28-a4d5-fd6298ea0a07" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/ffd07317-b643-4f25-8107-4b923e46a9ef" />
<img height="240" alt="image" src="https://github.com/user-attachments/assets/8d6e96a2-4e42-4a93-ae01-04f0f03b064b" />





~~~
인증 이미지가 없으면 과제 수행으로 인정되지 않습니다.
~~~



### 🎉 수고하셨습니다.
