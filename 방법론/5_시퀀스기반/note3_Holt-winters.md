# 시퀀스 기반 이상탐지 - Holt-winters

</br>
</br>

> ## 정의
- Exponential smoothing(지수평활) 기법을 기반으로 Trend와 Seasonality 속성을 추가해 시계열 데이터를 예측하고,    
  신뢰구간을 설정하여 이상 탐지하는 기법
- Exponentail smoothing
  * 시간적으로 가까운 데이터는 가중치를 높게, 오래된 데이터는 가중치를 낮게 설정하여 미래의 값을 예측하는 방법
- 단순한 지수평활 기법에 trend와 seasonality 속성을 추가함
- Holt-winters 모델을 통해 시계열 데이터를 예측하고, brutlag 알고리즘을 활용해 신뢰구간을 설정 -> 이를 넘으면 이상치로 간주
- 단순한 구조이지만 여러 과제에서 baseline model로서 널리 활용되고 있음

![image](https://user-images.githubusercontent.com/55543156/221913393-accb1287-9907-4044-978b-370ff8b3fb91.png) </br>
(파란색은 실제 데이터, 주황색은 예측값, 빨간색은 신뢰구간)

</br>
</br>

> ## 장단점

|장점|단점|
|:---|:---|
|- 장기간 예측 가능 </br> - 하이퍼파라미터 조정하여 이상 허용오차(신뢰구간) 유연하게 대응 </br> - 연산량이 적음. 큰 데이터셋에 대해 리소스 절약, 자동화 가능 </br> - 계절성을 고려한 이상탐지 가능. 곱셈분해 가능.(STL과 차이) |- 단변량 데이터에 대해서만 적용 가능, 상관관계 고려 불가 </br> - 계절성이 없는 데이터에 대해서는 성능 저조함 </br> - 변동이 적은 계절성 데이터에 대해서는 민감하게 탐지할 우려가 있음 |


</br>
</br>

> ## 사용방법
- statsmodels 라이브러리에 내장
  - from statsmodels.tsa.holtwinters import ExponentialSmoothing
1) Exponential Smoothing을 통해 전체 데이터셋 학습하고 예측
2) Brutlag Algorithm 활용해 신뢰구간 설정
3) 신뢰구간 기반으로 이상 Point 탐지

</br>
</br>

> ## 현업사례
- 시간에 종속된(추이나 계절성 존재) 데이터의 이상 탐지
- 수요 예측 

</br>
</br>

> ## 코드

```python
from statsmodels.tsa.holtwinters import ExponentialSmoothing

temp_df = co2
train_df = co2

""" (1) ExponentialSmoothing 모델을 만들어 학습, 전체 데이터에 대해 예측 """
# additive는 경향성이 일정함을 의미하고, 경향성 변동폭이 있을 때는 multiplicative를 사용
model = ExponentialSmoothing(
    train_df, trend='additive', seasonal='additive').fit()
    
prediction = model.predict(
    start=temp_df.index[0], end=temp_df.index[-1])

""" (2) Brutlag Algorithm 으로 신뢰구간 설정 """
PERIOD = 12        # The given time series has seasonal_period=12 
GAMMA = 0.4        # the seasonility component (신뢰구간 설정시 가까이 있는 데이터에 가중치 주는 scaler 역할)
SF = 1.96          # brutlag scaling factor for the confidence bands. (95% 구간)
UB = []            # upper bound or upper confidence band (신뢰구간 상한)
LB = []            # lower bound or lower confidence band (신뢰구간 하한)

# 실측치와 예측치를 비교하는 자료구조
difference_array = []
dt = []
difference_table = {"actual": temp_df, "predicted": prediction, "difference": difference_array, "UB": UB, "LB": LB}

# brutlag 알고리즘
# 12개월 이전의 실측/예측 차이에 0.4, 이번달 차이에 0.6 정도의 가중치를 주어 저장
for i in range(len(prediction)):
    diff = temp_df.iloc[i]-prediction.iloc[i]
    if i < PERIOD: # 12개월 전 데이터가 없을 땐 0.6 곱함
        dt.append(GAMMA*abs(diff))
    else:
        dt.append(GAMMA*abs(diff) + (1-GAMMA)*dt[i-PERIOD]) # 12개월 이전 차이에 0.4 곱함

# 저장된 실측/결측 차이를 예측치의 95% 신뢰구간(1.96)으로 반영하여 Upper/Lower Band 계산
    difference_array.append(diff)
    UB.append(prediction[i]+SF*dt[i])
    LB.append(prediction[i]-SF*dt[i])
    
"""Classification of data points as either normal or anomaly"""
normal = []
normal_date = []
anomaly = []
anomaly_date = []

# 신뢰구간을 벗어나는지 판단하여 normal, anomaly 결정
for i in range(len(temp_df.index)):
    if ((UB[i] <= temp_df.iloc[i]).bool() or (LB[i] >= temp_df.iloc[i]).bool()) and i > PERIOD:
        anomaly_date.append(temp_df.index[i])
        anomaly.append(temp_df.iloc[i][0])
    else:
        normal_date.append(temp_df.index[i])
        normal.append(temp_df.iloc[i][0])
        
anomaly = pd.DataFrame({"date": anomaly_date, "value": anomaly})
anomaly.set_index('date', inplace=True)
normal = pd.DataFrame({"date": normal_date, "value": normal})
normal.set_index('date', inplace=True)

# plotting
plt.figure(figsize=(24,12))
plt.plot(normal.index, normal, 'o', color='green', markersize=5)
plt.plot(anomaly.index, anomaly, 'o', color='red', markersize=5)
plt.plot(temp_df.index, UB, linestyle='--', color='grey')
plt.plot(temp_df.index, LB, linestyle='--', color='grey')
plt.legend(['Normal', 'Anomaly', 'Upper Bound', 'Lower Bound'])
plt.show()
```
