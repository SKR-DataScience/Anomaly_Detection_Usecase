# 시퀀스 기반 이상탐지 - S-H-ESD

> ## 정의 및 개념설명

### 공식문서
- [트위터 기술 블로그](https://blog.twitter.com/engineering/en_us/a/2015/introducing-practical-and-robust-anomaly-detection-in-a-time-series)
- 트위터에서 발표한 논문: [Automatic Anomaly Detection in the Cloud Via Statistical Learning](https://arxiv.org/pdf/1704.07706.pdf)
- 모듈 구현 코드: [R(original)](https://github.com/twitter/AnomalyDetection), [Python](https://github.com/zrnsm/pyculiarity)


### 강의설명
- S-H-ESD: Seasonal Hybrid ESD, 시계열 동적 이상탐지(Contextual Anomaly), 트위터에서 개발한 알고리즘
- 통계 알고리즘 별 단점을 보완, 종합해 만든 시퀀스 대상 이상탐지 알고리즘
- 평균, 표준편차 기반 이상탐지의 단점
    - outlier를 포함해 계산돼 이상값에 취약
    - 평균과 표준편차가 계절성, 트렌드에 의해 변화해 outlier를 놓칠 수 있음
- 핵심개념
    1. MAD(Medain Absolute Deviation): 관측값에서 중앙갑을 뺸 값들의 중앙 값, mean 대신 median을 사용해 이상치에 덜 영향받음(robust)
    2. ESD test(Grubb's test): (다변량이 아닌)단일 이상치를 테스트하는 통계적 방법, 평균에서 얼마나 멀어져있는지를 데이터의 분산과 대비해 탐색
        - outlier를 한 번에 하나만 탐색 가능
    3. Generalized ESD: 여러개 outlier 탐색, 계절성 고려 안함
    4. STL: 시계열 데이터에서 계절성, 추세, 잔차 세가지 요소로 분해, 이상탐지에 적합한 잔차만 고려

### 재설명
- 이 알고리즘은 이름 그대로 Seasonal Hybrid "ESD(extreme Studentized deviate)"이다. 즉 t-분포 기반으로 t-test를 통해 이상탐지를 하되 seasonality를 고려하는 것이다.
1. Generalized ESD? [참고](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h3.htm)
    - ESD가 한 개의 t-test를 통해 한 개의 outlier를 판단한다면 generalized ESD는 여러 개의 outlier를 판단한다.
    - 여느 가설검증이 그렇듯, 데이터 분포에 가정이 따르는데 ESD는 데이터의 정규성을 가정한다.
    - 귀무가설(H0): 데이터에 outlier가 없음
    - 대립가설(H1): 데이터에 r개의 ouliter가 있음
    - 검정통계량 $R_i = \frac{max_i|x_i-\bar{x}|}{s}$, 
        - 여기서 $\bar{x}$는 표본들의 평균, $s$는 표본들의 표준편차를 뜻한다.
        - $R_i$>기각역(기각역 식은 위 참고 페이지 확인)이면 i번째 데이터는 이상치다. $R_i$는 $t$분포에 관한 식(정확히는 $t_{\alpha/(2N), N-2}$에 관한 식, $N$은 데이터 수)인데 얼마나 엄격한 잣대로 이상치로 판단하느냐(임계치 $\alpha$ 수준)에 따라 기각역이 달라진다. 
        - 첫번째 이상치를 탐지하면 이 데이터를 제거하고 $R_i$를 구하고, 두번재 이상치를 탐지하면 두 개의 이상치를 제거하고 $R_i$를 제거한다. 대립가설에서 설정한 r개의 이상치를 탐지하기 위해 이 과정을 r번 반복한다.
2. ESD 테스팅을 통한 이상탐지 대상? <br/>
$\rightarrow$ 시계열분해(STL)를 통해 추출된 계절성을 뺀 값
    - "$x_i$ - seasonality(시계열분해를 통해 구함) - $median(x)$"를 ESD 테스트한다.
    - STL은 시계열을 계절성(특정 사이클마다 반복)+트렌드(우상향 혹은 우하향)+잔차로 분해하는 것을 일컫는데 자세한 것은 뒤 챕터에서 설명하도록 한다.


> ## 장단점
- 이상탐지 가능한 경우 
    - 잘 탐지하는 경우
        - 사이클(주기)가 있는데 급락, 급등이 있는 경우
    - 탐지 어려운 경우
        - 시간 검출단위가 균등하지 않거나 빈도가 너무 높은 경우
        - 급격한 하강, 이질적 신호가 섞임
        - 추세/계절성 추출이 어려운 경우
        - 평면적(flat) 데이터
        
|장점|단점|
|:---|:---|
|1. 노이즈의 증가<br/>2. 급등(sudden grow, spike) <br/>3. 하강(break down) <br/>4. 보이지 않는 회귀값(예상치 못한 관측값, activity when usually none) |1. 점진적 증가 신호(seasonal grow) <br/>2. 평면 신호(flat signal) <br/>3. 점진적 증가하는 신호에서 음의 방향 이상치(negtative seasonal anomaly)|

- 파이썬 패키지 사용시 특징
    - 시간 데이터 타입 변경
    - 시각화, 클러스터링에 사용 가능

> ## 현업사례
- SNS 이상탐지

> ## 코드
- 데이터: 첫번째 열은 시간, 두번째 열은 시퀀스로 이루어진 데이터프레임, 단 시간 열은 int64의 숫자형태
- 파라미터
     - max_anoms: 이상치 비율(전체 데이터 대비 비율)
     - direction: 이상치 판단 방향('pos' | 'neg' | 'both'), spike인지 shock인지

~~~python
from pyculiarity import detect_ts
from pyculiarity.date_utils import date_format

# time을 숫자형태(int64)로 변환, datetime 형태라면 계산되지 않음
data['time'] = np.int64(data['Time'])

# 이상치 추출
results = detect_ts(data, max_anoms=0.30, direction='pos')

# 결과값
# {'anoms': 이상치로 판단된 row들의 dataframe, 'plot':None}
~~~
