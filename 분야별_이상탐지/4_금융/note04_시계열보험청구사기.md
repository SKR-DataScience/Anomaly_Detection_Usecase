# 시계열 보험 청구 사기

> ## 문제 정의
- 과거 고객의 행동 패턴을 추적하여, 보험 사기 감지 모델을 개발


> ## 보유 데이터 & 특징

|구분|컬럼 구성|비고|
|:---|:---|:---|
|예측타겟|||
|인구 통계|성별, 나이, 기혼여부||
|보유 차량|차량 메이커, 차량 카테고리, 차량 연식, 소유 차량 대수||
|사고|사고지역, 과실 책임, 청구 날짜, 사고 이후 날짜, 사고 신고 여부|
|보험 상품|보험 형태, 보험 계약자 부담금액, ||
|과거 보험 이용 패턴|운전자 등급, 과거 보험 청구 횟수, 과거 사기 여부||


> ## 성과 측정
- 사기 검거율

> ## 데이터 처리 주의사항
1. 삼각함수를 통한 시계열 주기성 변수 생성
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
    - 순서를 고려하지 않는 카테고리
    - 순서가 있는 카테고리(심도 고려)
~~~
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

> ## 해당 강의 회차 종합 평가
-