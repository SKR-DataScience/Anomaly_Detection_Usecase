# 시퀀스 기반 이상탐지 - STL

</br>
</br>

> ## 정의
- 시계열 데이터 분해 후, Residual(잔차)를 이용하여 데이터 이상을 탐지하는 기법
- 시계열 분해?
  * 시계열 데이터를 여러 개의 구성요소로 분해하는 통계적 방법.   
    시계열 데이터가 '체계적 성분'과 '불규칙적 성분'으로 구성된다고 가정 -> 이를 분리하여 시계열 데이터를 분석/예측하는 것이 목적.   
  * 체계적 성분은 '추세 성분','계절 성분','순환 성분'이 있으며, 이러한 요소로 설명할 수 없는 오차(Noise)가 불규칙적 성분에 해당.
    * 추세 성분(Trend): 데이터가 장기적으로 증가하거나 하락하는 흐름. 꼭 선형적이지 않아도 됨.   
    * 계절 성분(Seasonality): 데이터에서 나타나는 명확하고 일정한 패턴. 정확히 일정한 빈도로 나타남. (매년 특정한 시기, 1주일, 특정 요일 등)    
    * 순환 성분(Cycle): 고정된 주기나 빈도는 아니지만, 반복적으로 증가하거나 감소하는 패턴. (경제 상황으로 인한 경기 순환 등)
  * 기존 다양한 시계열 분해 기법이 존재함 (Classical Decomposition, X11, SEATS 등)

  * STL에서는 Trend와 Seasonality를 제거하고 남은 Residual을 이용해 이상치 탐지
  
    ![image](https://user-images.githubusercontent.com/55543156/221077277-5cd029a6-e8ed-453d-80cd-330e896770aa.png)

- 특징
  * 다양한 상황에서 사용할 수 있는 강력한 시계열 분해 기법
  * 월별, 분기별 포함 어떤 종류의 계절성도 다를 수 있음 (SEATS, X11과의 차이)
  * 계절적인 성분이 시간에 따라 변 해도, Hyper parameter를 통해 반영 가능


</br>
</br>

> ## 장단점

|장점|단점|
|:---|:---|
|- 분기별, 월별, 일별 분해 모두 가능 </br> - Moving Avg 방식으로 분해하지 않아 데이터 유실 없음 </br> - 돌발 이상치에 대해 추세,주기 영향 받지 않음 |- 캘린더 데이터를 반영하지 못함. 일별 데이터로 변환 필요 </br> - 덧셈 분해 기능만 제공 |

  cf) 덧셈 분해 / 곱셈 분해 : Trend 변화에 따른 데이터 변동폭을 고려한 전통적인 시계열 분해 기법

  * 덧셈 분해(additive decomposition) : y = S + T + R 
    * Trend가 변화함에도 데이터의 변동폭이 일정하면 덧셈 분해를 사용
      (Trend와 Seasonal의 관계가 없다고 판단될 때)

  * 곱셈 분해(multiplicative decomposition) : y = S * T * R
    * Trend가 변화함에 따라 데이터의 변동폭도 변화하면 곱셈 분해를 사용
      (Trend와 Seasonal 사이에 관계가 있다고 판단될 때)
    
    ![image](https://user-images.githubusercontent.com/55543156/221903590-cf1840b6-1bf2-4c39-b875-103e476e2bdb.png)
   
    * 왼쪽 데이터는 덧셈 분해가, 오른쪽 데이터는 곱셈 분해가 적절함  


</br>
</br>

> ## 사용방법
- statsmodels 라이브러리에 내장
  - from statsmodels.tsa.seasonal import STL
- 주요 파라미터로 주기(Seasonal) 설정
- Seasonal 및 Trend 제거 후 남은 Residual에 대해서, 정규성 검증 및 3-sigma rule 적용해 이상 탐지

</br>
</br>

> ## 코드

```python
from statsmodels.tsa.seasonal import STL

# Odd num : seasonal = 13(연도별) / seasonal = 5(분기별) / seasonal = 7(주별) <<홀수로 넣어야 함
stl = STL(co2, seasonal=13)
res = stl.fit()

res.trend # 추세 확인
res.seasonal # 계절성 확인
res.resid # 잔차 확인

# Ztest를 통한 정규성 검증(Nomality Test)
# Ztest : 정규분포를 가정하며, 추출된 표본이 동일 모집단(정규분포)에 속하는지 가설 검증하기 위해 사용 (※ p-value가 0.05 이상이면 정규성을 따름)
from statsmodels.stats.weightstats import ztest
r = res.resid.values
st, p = ztest(r)
print(st,p)  

# ▶ 평균과 표준편차 출력
mu, std = res.resid.mean(), res.resid.std()
print("평균:", mu, "표준편차:", std)

# 3-sigma(표준편차)를 기준으로 이상치 판단
print("이상치 갯수:", len(res.resid[(res.resid>mu+3*std)|(res.resid<mu-3*std)]))

# ▶ 이상 데이터 확인
co2[res.resid[(res.resid>mu+3*std)|(res.resid<mu-3*std)].index]
```

