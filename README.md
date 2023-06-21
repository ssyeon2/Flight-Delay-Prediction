# 월간 데이콘 항공편 지연 예측 AI 경진대회
- 일부 레이블만 주어진 학습 데이터셋을 이용한 항공편 지연 여부 예측
- 레이블 없는 데이터와 함께 항공편 지연 여부 예측하는 AI 모델 개발
<br>
<br>

**데이터** <br>
- https://dacon.io/competitions/official/236094/overview/rules
- 외부 데이터 사용 금지
<br>

**평가** <br>
![image](https://github.com/ssyeon2/Flight-Delay-Prediction/assets/105052724/f2283b00-adfd-438a-80f4-81e8ab6b0c10)
- 심사 기준: LogLoss
- p는 target(Delayed)에 대한 확률값, 1-p는 Not_Delayed에 대한 확률값
- LogLoss
  - 분류 문제의 대표젓인 평가지표로 교차 엔트로피(cross-entropy)라고도 함
  - 분류 모델 자체의 잘못 분류된 수치적인 손실값(loss)을 계산
  - logloss는 낮을수록 좋은 지표
  - 로지스틱 회귀모렏이서의 손실 함수


## 해결방안
### 1. EDA <br>
**대응 변수 확인하기**
1) 1:1대응 <br>
   (1) Origin_Airport, Origin_Airport_ID <br>
   (2) Destination_Airport, Destination_Airport_ID <br>
   (3) Airline, Carrier_ID(DOT) <br>
   -> 둘 중 하나의 변수는 drop <br>
2) 1:N대응 <br>
   (1) Origin_Airport, Origin_State <br>
   (2) Destination_Airport, Destination_State <br>
3) 대응 관계 없음 <br>
   (1) Airline, Tail_Number <br>
   (2) Airline, Carrier_Code(IATA) <br>

### 2. 결측치 처리 <br>
1) Estimated_Departure_Time, Estimated_Arrival_Time 변수
   -> KNNImputer 사용
2) _Airport → _State 변수
   -> N:1 대응으로 결측치 대체
3) Carrier_ID → Airline 변수
   -> 1:1 대응으로 결측치 대체
4) 대응으로 채울 수 없는 값
   - train -> 행 삭제
   - test -> 최빈값


### 3. 비지도 학습 <br>
**selftraining** <br>
   - [최고 성능] randomforest <br>
   - catboost <br>
   - svm <br>
   
<br>
