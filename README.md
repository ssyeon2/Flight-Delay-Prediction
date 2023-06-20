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
### 1. 결측치 처리
- KNNImputer
- 1:1 대응

### 2. 준지도 학습
1) selftraining <br>
   - catboost <br>
   - randomforest <br>
   - svm <br>
2)
   - 
   
<br>
