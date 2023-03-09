# 통신사 이탈 유저 예측

> ## 문제 정의
- 통신사 유저의 이탈율을 예측해 이탈이 예상되는 고객에게 새로운 오퍼를 제공하고 고객을 유지하고자 함

> ## 보유 데이터 & 특징

[커머스 & 구독 데이터 특징]

- 가입 여부와 같이 binary 컬럼이 많음
- 이용 기간이 중요
- 데이터를 구간화해서 검증할 일이 빈번
    - 나이 구간 별 이탈률
    - 가입 기간 구간 별 이탈률

|구분|컬럼 구성|비고|
|:---|:---|:---|
|예측타겟|이탈여부|binary, 1 or 0|
|Demo(인구통계학적 특징)|성별, 나이, 기혼 여부||ß
|서비스 가입 정보|통신서비스 외 인터넷, TV 스트리밍 등||
|보험 가입 정보|기기 보험 가입 여부||
|가입기간|총 가입 기간, 약정 기간|통신사 이용 유형 확인 가능 <br/>- 충성고객, 통신사 자주 바꾸는 고객 등|
|사용 금액|이용 금액||

> ## 분석 단계 별 고려 사항
### 가설검증
- 유저 


### 모델 적합


> ## 주요 코드

~~~python
from pycaret.classification import ClassificationExperiment

experiment_name = 'AD_commerce04'
exp = ClassificationExperiment()

exp.setup(
    data=dataset,
    target=target_value,
    train_size=0.8,
    categorical_features=categorical_features,
    numeric_features=numerical_features,
    normalize=True,
    fold_strategy='kfold',
    fold_shuffle=True,
    fold=5,
    experiment_name=experiment_name
)
~~~