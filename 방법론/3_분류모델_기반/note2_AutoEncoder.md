# 분류 모델 기반 이상탐지 - AutoEncoder

> ## 정의
 <center><img src= "./fig2.png" width="75%"></center> <br/>
 <center>그림1. AutoEncoder 구조</center>

<br>

- 오토인코더: 비지도학습이지만, 정상 데이터가 필요함(Semi라 보는 게 더 정확함)
- 입력을 출력으로 복사하는 신경망 → 값을 예측하는게 아닌 자기 자신을 예측함.
- 구조: 데이터 압축하는 부분이 인코더, 데이터 복원하는 부분이 디코더
- 차원을 줄이는 과정에서 핵심적인 부분이 추출이 되고(Latent Vector) 이걸 가지고 다시 input을 예측하는 것
- 고차원 데이터의 가장 중요한 특징을 학습함

<br>

> ## 오토인코더가 이상탐지에 쓰이는 방법
 - 정상적인 데이터로만 학습한다면, Latent Vector는 정상데이터의 특징을 가져올 것. 
   - 이렇게 학습된 알고리즘에 이상 데이터가 들어오면 자가복제가 잘 안 됨    
    → LOSS가 커지면 이상이라 탐지!
    

 - 오토인코더는 이상탐지에서 잘 활용된다!
   - 자기 자신을 예측하는 것이기 때문에 비선형적인 것도 Latent Vector가 잘 뽑아낼 수 있음
   - 활성화함수로 LINEAR를 사용하면 PCA와 똑같아짐

<br>

> ## 장단점
|장점|단점|
|:---|:---|
|1. 데이터 Label이 존재하지 않아도 사용 가능 <br/> but 정상적인 데이터를 추정할 수 있을 정도는 되어야 함 <br/> 2. 고차원 데이터 특징을 추출 가능 <br/> 3. 오토인코더 기반의 다양한 알고리즘 존재, 웹상에 많은 레퍼런스 존재 |1. 히든레이어 설정(하이퍼 파라미터) <br/> 2. Loss(MSE)에 대한 threshold 설정이 어려움|

- 단점 추가 설명
  - 히든레이어 설정 → 왜 가중치를 그렇게 설정했느냐?라는 질문에 그렇게 했더니 성능이 잘 나왔어요 라고 밖에 설명 못 함 
    - 성능은 좋지만 현업에 설명하기 어려운 알고리즘
  - Loss threshold 설정 → LOSS를 기준으로 이상을 탐지하는 알고리즘인데, 정확히 LOSS가 얼마를 넘었을 때부터 이상이라 할 것인지 정의하는 게 애매함. 
    - 감에 의해서 하거나 현업과 상의해서 하는데 어려움. 
    - 이상에 대한 레이블이 있다면 MSE가 어느정도까지 올라가는지 파악할 수 있어 훨씬 수월함


<br>

> ## 사용방법 
> 이상탐지에 특화된 오토인코더 라이브러리 존재 Pyod

- from pyod.models.auto_encoder import AutoEncoder
- 정상 데이터 확보
- 하이퍼파라미터 결정
- 학습 및 예측
- threshold 및 scoring

<br>

> ## 활용 예
- 시계열 센서 데이터 이상 탐지 
  -  센서데이터 이상탐지: 각 독립변수 MSE 값의 합을 봄 → 상호관계를 고려하지 않고, MSE의 총합만 보기 때문에 Correlation 관계는 제거해주는 게 좋음

<br>

> ## threshold 결정을 위한 modified Z점수 표준화
> 이상치에 robust한 표준화 방법
```
def mod_z(col):

    med_col = col.median()

    med_abs_dev = (np.abs(col - med_col)).median()

    mod_z = 0.7413 * ((col - med_col) / med_abs_dev)

    return np.abs(mod_z)

Z점수 표준화를 통해 어떻게 분포되어있는지 먼저 확인하고, 어떤 지점 넘어가면 아웃라이어로 하겠다라고 설정

```
