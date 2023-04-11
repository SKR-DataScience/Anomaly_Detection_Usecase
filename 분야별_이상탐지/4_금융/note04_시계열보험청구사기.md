# 시계열 보험 청구 사기

> ## 문제 정의
- 과거 고객의 행동 패턴을 추적하여, 보험 사기 감지 모델을 개발


> ## 보유 데이터 & 특징

|구분|컬럼 구성|비고|
|:---|:---|:---|
|예측타겟|사기보험청구 여부|binary|
|인구 통계|성별, 나이, 기혼여부||
|보유 차량|차량 메이커, 차량 카테고리, 차량 연식, 소유 차량 대수||
|사고|사고지역, 과실 책임, 청구 날짜, 사고 이후 날짜, 사고 신고 여부|
|보험 상품|보험 형태, 보험 계약자 부담금액, ||
|과거 보험 이용 패턴|운전자 등급, 과거 보험 청구 횟수, 과거 사기 여부||


> ## 성과 측정
- 사기 검거율

> ## 데이터 특징
- 변수, 데이터 수 모두 많음
- 사기(abnormal) 사례가 적은 불균형 데이터
- 시계열적 특징을 모델에 반영하고자 함
    - 고려 변수: 사고일, 청구일, 사고일과 청구일 간의 시간 차
    - 인사이트: 사고일의 월, 일, 요일 별 사고 건수 및 사기(abnormal) 건수 비교 <br/> $\rightarrow$ 특정 주기에 사고가 많음

> ## 데이터 처리 특이사항
1. 삼각함수를 통한 시계열 주기성 변수 생성
    - 12월과 1월은 연속적이지만 12와 1 값으로 나타낸다면 두 달의 인접성을 표현하기 어렵다. 또는 카테고리 변수로 표현하면 항목 N개에 대해 N-1개의 변수가 새로 생성되어 변수 차원이 급격히 증가한다. 이를 해결하기 위 삼각함수를 이용해 주기 변수를 표현한다.  
~~~
import numpy as np

data = (
    data
    .assign(Date_Accident_Month_Cos=lambda x : np.cos(x['Date_Accident_Month']*2*np.pi/12))
    .assign(Date_Accident_Dayofmonth_Cos=lambda x : np.cos(x['Date_Accident_Dayofmonth']*2*np.pi/31))
    .assign(Date_Accident_Weekofmonth_Cos=lambda x : np.cos(x['Date_Accident_Weekofmonth']*2*np.pi/4))
    .assign(Date_Accident_Month_Sin=lambda x : np.sin(x['Date_Accident_Month']*2*np.pi/12))
    .assign(Date_Accident_Dayofmonth_Sin=lambda x : np.sin(x['Date_Accident_Dayofmonth']*2*np.pi/31))
    .assign(Date_Accident_Weekofmonth_Sin=lambda x : np.sin(x['Date_Accident_Weekofmonth']*2*np.pi/4))
    .assign(Date_Claimed_Month_Cos=lambda x : np.cos(x['Date_Claimed_Month']*2*np.pi/12))
    .assign(Date_Claimed_Dayofmonth_Cos=lambda x : np.cos(x['Date_Claimed_Dayofmonth']*2*np.pi/31))
    .assign(Date_Claimed_Weekofmonth_Cos=lambda x : np.cos(x['Date_Claimed_Weekofmonth']*2*np.pi/4))
    .assign(Date_Claimed_Month_Sin=lambda x : np.sin(x['Date_Claimed_Month']*2*np.pi/12))
    .assign(Date_Claimed_Dayofmonth_Sin=lambda x : np.sin(x['Date_Claimed_Dayofmonth']*2*np.pi/31))
    .assign(Date_Claimed_Weekofmonth_Sin=lambda x : np.sin(x['Date_Claimed_Weekofmonth']*2*np.pi/4))
)
~~~

2. 카테고리 변수 인코딩 방식
    - 보험 청구 횟수 변수를 다음 두 가지 형식으로 표현 가능
        - 순서를 고려하지 않는 카테고리
        - 순서가 있는 카테고리(심도 고려)
    - 해당 예제의 경우 카테고리 값의 심도(순서)를 고려한 변수를 포함할 때 예측 성능이 나아짐

~~~ python
(
    data
    .groupby('PastNumberOfClaims')
    ['FraudFound']
    .value_counts()
    .to_frame()
    .rename(columns={'FraudFound':'Count'})
    .reset_index(drop=False)
    .loc[lambda x : x['FraudFound']=='Yes']
    .sort_values(by='Count')
    .assign(Ordinal_Encoded=list(range(1,5)))
)
~~~

> ##  알고리즘 선택
- pycaret 패키지를 이용해 다중 알고리즘을 한꺼번에 적합

~~~python
from pycaret.classification import ClassificationExperiment

experiment_name = 'chapter_22_ordinal_encoding'
exp = ClassificationExperiment()

dataset = data.loc[:, ['VehicleCategory', 'PastNumberOfClaims', 'FraudFound']]
target_value = 'FraudFound'

'''
version1 카테고리 변수
'''
exp.setup(
    data=dataset,
    target=target_value,
    train_size=0.8,
    categorical_features=['VehicleCategory', 'PastNumberOfClaims'],
    normalize=True,
    fold_strategy='kfold',
    fold_shuffle=False,
    fold=5,
    experiment_name=experiment_name
)
'''
version2 카테고리 변수 - 순서 고려
'''
ordinal_features = {
    'PastNumberOfClaims' : [
        'more than 4',
        '1',
        '2 to 4',
        'none'
    ]
}
exp.setup(
    data=dataset,
    target=target_value,
    train_size=0.8,
    categorical_features=['VehicleCategory'],
    ordinal_features=ordinal_features,
    normalize=True,
    fold_strategy='kfold',
    fold_shuffle=False,
    fold=5,
    experiment_name=experiment_name
)

# 특정 알고리즘만 지정
# model = exp.create_model('lr')

# 여러 알고리즘 한꺼번에 적합
## 다양한 분류기: 트리, LDA, QDA, Logistic Regression, KNN, SVM, Ridge
models = exp.compare_models(sort='AUC', n_select=5)

'''
성능확인
'''
model = models[0]
# 혼돈 행렬
exp.plot_model(model, plot='confusion_matrix', plot_kwargs={'percent':True})
# AUC
exp.plot_model(model, plot='auc')
# ROC의 threshold
exp.plot_model(model, plot='threshold')
# 학습 곡선
exp.plot_model(model, plot='learning')
# validattion 곡선
exp.plot_model(model, plot='vc')
# 분류 decision boundary 시각화
exp.plot_model(model, plot='boundary')
# 변수 중요도
exp.plot_model(model, plot='feature')

'''
test set 성능 확인
'''
# 최종 모델 결정
model_finalized = exp.finalize_model(model)

x_test = exp.X_test
y_test = exp.y_test

from sklearn.metrics import roc_auc_score

yhat_test = model_finalized.predict(x_test)
roc_auc_score(
    yhat_test.apply(lambda x : 1 if x=='Yes' else 0),
    y_test.apply(lambda x : 1 if x=='Yes' else 0),
)

'''
최종 모델 저장
'''
exp.save_model(
    model=model_finalized, 
    model_name=f'model_{experiment_name}_v1'
)
~~~


> ## 해당 강의 회차 종합 평가
- 시계열 분해에서 삼각함수를 통해 주기를 표현하므로 삼각함수를 통해 요일 등의 변수를 표현하는 것은 괜찮아 보임
- pycaret을 이용해 데이터의 기본적인 성능을 보는 것은 빠르게 모델의 baseline 성능을 가늠해볼 때는 괜찮아 보임. 그러나 데이터의 특징이 모델의 스트럭쳐 구축에 전혀 고려되지 않음, 1회차 분석 이후 2회차 모델을 개선할 때 이 점이 반영되어야 함.
- 고객 개개인 별 보험 청구 및 차량 운행 히스토리를 파생변수로 만들어 추가 필요