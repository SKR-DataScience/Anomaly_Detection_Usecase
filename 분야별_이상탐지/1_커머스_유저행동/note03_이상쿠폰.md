# 이상쿠폰 
- 이상 레이블이 없는 경우, EDA를 통해서 어떻게 모델을 만들어나가는지, 
- 이상 레이블이 없을 때 사용할 수 있는 방법들은 무엇이 있는지 알아봄

<br>

> ## 시나리오
- A회사는 마케팅 캠페인의 일환으로 바우처/쿠폰을 발행했고, 이에 대한 데이터를 보유하고 있음
- 여러 정황으로 보았을 때 같은 유저가 반복해서 사재기를 하거나, 도매에서 남용된 쿠폰 등이 있는 것으로 의심됨
- 이를 잡아내는 Rule-based model을 개발하여 다음 마케팅 캠페인 때 제대로된 타겟 유저들에게만 쿠폰이 전달/사용 될 수 있도록 조치하려고 함


<br>

> ## 문제 해결 프로세스 정의
- 문제정의
    - 과거 쿠폰 사용 데이터로부터 EDA를 통해 남용 의심 유저에 대한 Rule-based model 개발 
  (이상 label이 없으니, 우리가 룰을 만들어야 함)
- 기대효과
    - 마케팅 비용 절감 및 동일 비용 대비 효과 극대화
- 해결방안
    - 과거 사고 데이터를 바탕으로 쿠폰 남용 탐지 모델 개발
        - Session 1 - 데이터 전처리 및 EDA
        - Session 2 - 가설 수립 및 검증, 인사이트 발굴
        - Session 3 - 이상 탐지 모델링 
        1) rule-based model 만들기
        2) 클러스터링(unsupervised) 방법 사용 
        3) 룰을 만들면 레이블 할 수 있음 → 해당 데이터로 학습하고 새 데이터는 머신러닝으로 예측
- 성과측정
    - 마케팅 비용 비교 / 소비자 Retention 등
- 현업적용
    - 모델에 인풋하기 위한 데이터마트 생성
    - 예측 모델 활용을 위한 사고 데이터 수집

<br>

> ## Session 1 - 데이터 전처리 및 EDA
- 데이터 이해
    
    (1) Data shape 확인 - row, column 수
    
    (2) Data type 확인 - categorical 변수가 numerical로 잘못 들어가있지는 않은지
    
    (3) Null 값 확인 
    
    - 빈 값이 있을 경우 모델에서 null 값 핸들링해주는지 확인
    - 또는 imputation을 통해 빈 값을 채워주거나
    - imputation이 어려울 경우 해당 row를 제거하는 방법도 고려해야 함
    → missingno 패키지 활용!
    
    (4) # of unique values, data distribution, outlier 확인 (정상적인 범주를 벗어난 데이터)
    
    - Number of unique values: 예측에 사용할 수 있는 유의미한 정보를 담은 컬럼인지, 단순id 등 의미없는 컬럼인지 확인
    - Distribution: 학습에 방해가 될 수 있는 불균형한 데이터셋인지 확인. label이 imblanced한 경우 novelty detection 등의 모델을 고려해야 함
    - Outlier 확인: 모델 학습에 방해가 될 수 있는 outlier 등이 존재하는지 확인
- EDA로부터 얻은 인사이트
    - 피쳐수가 많으나 활용할 수 있는 데이터인지는 검증 필요
    - 데이터에 결측치 존재
    - 중복된 정보로 가입한 유저 다수 존재할 것으로 추정
    - IP, EMAIL등 원형태 그대로는 활용이 어려운 데이터가 있어 추가 처리 필요


<br>

> ## Session 2 - 가설 수립 및 검증, 인사이트 발굴
- 가설 수립하는 방법
    - EDA: 데이터를 보고 떠올리거나
    - 문헌조사: 관련 논문, 업계 리포트, 뉴스 등을 참조하거나
    - 인터뷰: 관련 부서 담당자분들께 아이디어를 얻음
    
- 위에서 뽑아낸 가설
    - 중복 정보로 가입한 유저들은 남용일 확률이 높다
    - IP로부터 지역정보를 알아내거나, E-mail domain 등으로도 중복 정보를 추가로 추려낼 수 있는지?
    - Device, Sim ID 등에서 유효한 정보를 추가로 알아낼 수 있는지?

<br>

> ## Session 3 - 이상 탐지 모델링
1) Rule-based model

    <center><img src= "./.png" width="75%"></center> <br/>
    <center>그림1. Rule-based model</center>

   - 특정한 규칙을 지정해, 해당 규칙에 해당하는 데이터를 참/거짓으로 예측
   - 본 실험에서는, training set에서 사용된 적이 있는 개인정보가 test set에도 존재하는 경우 abuse인 것으로 예측하도록 정의
   - 모델이 abuse라고 예측한 데이터에서 중복기입 컬럼과 값을 뽑아내서 확인 → 결과가 합당한지 보면서 계속 룰 수정

2) Anomaly detection model - unsupervised
   - pycaret 라이브러리 사용 
   - 과거에는 Scikit-learn, Pytorch 등의 패키지 등을 통해 모델과 전/후처리 파이프라인들을 개별적으로 설정하여 모델 비교를 위한 실험 코드를 작성
   - 여러 프로젝트에서 동일한 코드를 반복적으로 작성/사용하게 되며 이로 인해 효율이 떨어짐
   - 이미 알려지고 사용되고 있는 모델들을 한꺼번에 실험할 수 있는 인터페이스 필요 → Pycaret 활용하여 빠르게 여러 모델간의 성능 비교 가능


3) Classification with labeled dataset

   - 1)에서 정한 rule 기반으로 우리가 target 값을 만들어줌
       - 값이 중복으로 등장하면 1, 아니면 0
   - 이 방법이 rule-based와 다른 점
       - rule을 통해 labeling을 하고, 그 데이터를 학습하여 이상을 탐지하는 것
       - 장점: 지정하지 않은 rule도 학습해서 미래 데이터가 들어왔을 때 우리가 생각하지 못했던 부분도 캐치할 수 있다.
       - 단점: 규칙을 정확하게 학습하지 못했을 경우 rule을 놓칠 수 있음
   - 여기에서는 LDA 성능이 제일 좋았다 (Linear discriminant analysis)
       - PCA: component axes that maximize the variance
           - 데이터를 가장 잘 설명하는 축을 찾으려 함
       - LDA: maximizing the component axes for class-separation
           - 어느 축으로 내렸을 때 분포가 잘 분리되는지(클러스터링이 잘 되는지)
