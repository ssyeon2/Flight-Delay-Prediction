# 월간 데이콘 항공편 지연 예측 AI 경진대회
- 일부 레이블만 주어진 학습 데이터셋을 이용한 항공편 지연 여부 예측
- 레이블 없는 데이터와 함께 항공편 지연 여부 예측하는 AI 모델 개발
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
<br>

**사용 툴**
- 협업 툴 : Notion <br>
  https://www.notion.so/2bf63c4ae47c487a8c4b1c13d2263c71?v=fd1ad63c456a432b912c29cc246937c6&pvs=4
- Google colab
<br>

**최종 결과 및 성과**
- LogLoss 최종 0.67236
- 상위 3% 전체 14등

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

### 3. 파생변수 생성
- Flight Total time : 도착시간 - 출발 시간
- Departure_Hour, Departure_Minute, Arrival_Hour, Arrival_Minute

### 4. 이상치
![다운로드](https://github.com/ssyeon2/Flight-Delay-Prediction/assets/105052724/545ddb66-a606-4510-b9e8-2536e38ba123)
- 로그 변환
- np.lg1p 사용

### 5. 사용 변수 추출
- 'Month'
- 'Origin_State'
- 'Origin_Airport'
- 'Destination_State'
- 'Destination_Airport'
- 'Distance', 'Airline'
- 'Total_Time'
- 'dep_hour'
- 'arr_hour'
- 'Tail_Number'
- 'Delay'

### 6. 스케일링
1. 범주화
   - 범주 컬럼이 너무 많은 관계로 get_dummies 사용시 Ram 부족 현상 발생
   - **해시 기반 특성 추출**(Hashing-based Feature Extraction)
       - 텍스트 또는 범주형 데이터를 숫자형태로 변환
       - 해시 함수를 사용하여 각 입력 값을 고정 크기의 특성 벡터로 변환
       - 입력 데이터의 내용에 기반하여 특성을 생성
       - 메모리 효율적인 특성 벡터화를 제공
2. Min-max scaling
   - 수치형 데이터에 한해 min-max scaling 진행

### 7. 비지도 학습
**selftraining** <br>
   - randomforest <br>
   - catboost <br>
   - svm <br>

### 8. 불균형 데이터 
![다운로드 (1)](https://github.com/ssyeon2/Flight-Delay-Prediction/assets/105052724/963fc5ef-6662-4df5-8b36-148d985ac57a)
- SMOTE 사용
- Oversamgpling하여 불균형 데이터 처리

### 9. 모델 학습
- LogLoss 지표에 성능이 좋은 LogisticRegresion 사용
- GridSearchCV 사용
- 최종 0.67236
- 상위 3%

