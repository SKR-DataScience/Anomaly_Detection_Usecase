# 거리기반 이상탐지 - LOF

</br>
</br>

> ## 정의
* 밀도 기반의 이상탐지 방법으로, 데이터 전역(Global)의 관점이 아닌 **국소적(Local) 정보를 이용하여** 이상치를 찾는 방법
  - KNN, 마할라노비스 등 기존의 거리기반 방법들이 local 정보에 대한 고려를 하지 않는다 단점을 극복하기 위함
  
  ![image](https://user-images.githubusercontent.com/55543156/219376604-c8228955-7cbe-4d0c-9d49-c387942e0e11.png)

  - 위 그림에서  전역적인 관점에서 보면 O1은 이상치, 그러나 O2는 이상치로 판단하지 못할 수도 있음   
    -> 국소적 정보를 고려하여 O2를 이상치로 판단해야 함
    
  - 'LOF' 측정해서 이상치 판단함 : '관측치가 주변 데이터로부터 얼마나 벗어나있는가'를 의미하는 abnormal score

</br>

> ### 근데 LOF 도출은 어떻게?

**[STEP 1]**
* d(p,q) : 관측치 p와 q의 거리
* k-distance(p) : 관측치 p와 k번째로 가까운 이웃의 거리
* Nk(p) : 관측치 p에 대해 k-distance(p)를 계산할 때 포함된 이웃의 갯수
  ![image](https://user-images.githubusercontent.com/55543156/219379583-85318823-79a4-4133-92cc-a8062f88b0e2.png) </br>
  (예시1) 3-distance(O) = 3, N3(O) = 3   
  (예시2) 3-distance(O) = 3, N3(O) = 4 (**3-distance(O)에 걸치는 이웃이 여러개면 모두 N3(O)에 포함됨)

**[STEP 2]**
* reach_distance k(p,o) : 에 대해 주변 데이터 o와의 거리를 계산하되, o의 k-distance를 고려해 보정한 거리

  ![image](https://user-images.githubusercontent.com/55543156/219381117-ef6fef50-9e68-49ee-98a2-57531c805076.png)
  ![image](https://user-images.githubusercontent.com/55543156/219381229-68a563dd-574b-4edc-8fd3-42f08a1a0170.png)
  
  - 관측치 p가 o에서 k-distance(o)보다 멀다면 : 관측치 p와 o의 실제 거리
  - 관측치 p가 o에서 k-distance(o)보다 가깝다면 : o의 k-distance
    (**나중에 밀도를 계산할 때, 데이터가 너무 촘촘히 붙어있을 경우 LOF 값이 너무 민감하게 변할 수 있어 보정하는 것)

**[STEP 3]**
* lrd k(p) : local reachability density of p. 관측치 p에 대한 k-이웃의 reach_distance k(p,o)의 평균의 역수

  ![image](https://user-images.githubusercontent.com/55543156/219384654-d73584e2-30c1-41b3-89cc-a4d9adaf1358.png)
  
  - k=3일 때 예시   
  ![image](https://user-images.githubusercontent.com/55543156/219384785-70f42935-5a26-4b1a-aea8-4ab01d62c8bc.png) </br>
  (예시1) p의 3-이웃이 가까이 있고, 이웃 Oi(i=1,2,3)의 reach-distance k(p,Oi)가 작음 -> 평균의 역수인 lrd 3(p)는 큰 값이 산출   
  (예시2) p의 3-이웃이 멀리 있고, 이웃 Oi(i=1,2,3)의 reach-distance k(p,Oi)가 큼 -> 평균의 역수인 lrd 3(p)는 작은 값이 산출   
  
  -> **★ lrd k(p)가 클수록, 관측치 p 주변 이웃의 밀도가 높다고 할 수 있음**   
  
**[STEP 4]**
* LOF k(p) : 관측치 p의 밀도(lrd k(p)) 대비 k-이웃 o의 밀도(lrd k(o))의 비율의 평균
  - 관측치 p의 관점에서뿐만 아니라 이웃 o관점에서도 lrd를 구한 다음, 그들을 비교하여 상대적 밀도를 계산
  
  ![image](https://user-images.githubusercontent.com/55543156/219386246-75d3f480-1089-4236-b510-69827116cb9c.png)
  
  - 예시   
  ![image](https://user-images.githubusercontent.com/55543156/219387181-2c3a899e-0f25-439b-9d61-5c33101c355d.png) </br>
  (case1) p,o가 모두 몰려있는 경우, lrd k(p)와 lrd k(O)가 모두 클 것이기 때문에 LOFk(p)는 1에 가까운 숫자가 됨 (정상)   
  (case 2) p만 떨어져있는 이상치인 경우, lrd k(p)는 작은데 lrd k(o)는 모두 클 것이므로 LOFk(p)는 1보다 큰 숫자가 됨 (이상)   
  (case 3) p,o가 모두 떨어져있는 경우, lrd k(p)와 lrd k(O)가 모두 작을 것이기 때문에 LOFk(p)는 1에 가까운 숫자가 됨 (정상)   
  
  - 정리하면,
    - LOF < 1 : 이웃 관측치에 비해 밀도가 높은 분포
    - LOF = 1 : 이웃 관측치와 비슷한 분포
    - LOF > 1 : 이웃 관측치에 비해 밀도가 낮은 분포 -> **크면 클수록 이상치 정도가 큼**

**[결과]** </br> 

  ![image](https://user-images.githubusercontent.com/55543156/219388552-eeac51cc-0544-404f-b25b-2c4b2bfb5ede.png)
  
 
</br>
</br>

> ## 장단점
* Local outlier를 찾을 수 있다는 장점이 있지만, 이상치 판단 기준이 복잡하다는 단점이 있음 (계산량도 증가)

|장점|단점|
|:---|:---|
|- 밀접한 클러스터에서 조금만 떨어져있어도 이상치로 탐지 가능 </br> **- Local Outlier를 탐지 가능** </br> - 데이터에 대한 가정이 필요 없음 </br> - KNN과 다르게 라벨링 없이도사용 가능 |- 데이터 차원 수가 증가할수록 계산량 많음 </br> - 이상치 판단 기준을 확실하게 설정하기 어려움 </br> (밀집도가 다른 여러 클러스터가 존재할수록 복잡해짐)  |


</br>
</br>

> ## 사용방법
* sklearn.neighbors 패키지 활용
* n_neighbors 결정 (이외 다양한 하이퍼파라미터 설정)
* 표준화 필수 (Min-max normalization, z-score standardization 등)
* 새로운 Data가 추가되면 지속적으로 수행


</br>
</br>

> ## 현업사례
* 공정(설비) 데이터 이상탐지
* 공정 센서 데이터 실시간 이상탐지


</br>
</br>

> ## 코드

```
from sklearn.neighbors import LocalOutlierFactor

# 모델 생성 (하이퍼파라미터 설정)
outlier = LocalOutlierFactor(n_neighbors=5, contamination=0.2)

# 모델 학습 및 예측
y_predict = outlier.fit_predict(df_train)
df_train['outlier'] = y_predict

# LOF 값 확인 ('negative_outlier_factor_'를 사용해 나오는 LOF값은 음의 방향으로 클수록 outlier일 가능성이 높음)
outlier.negative_outlier_factor_

# ▶ 3D plot 및 확인
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure(figsize=(9, 6))
ax = fig.add_subplot(111, projection='3d')

df_red = df_train[df_train['outlier']==-1]
df_green = df_train[df_train['outlier']==1]

ax.scatter(df_red['hour'], df_red['attendance'],df_red['score'], color = 'r', alpha = 0.5);
ax.scatter(df_green['hour'], df_green['attendance'],df_green['score'], color = 'g', alpha = 0.5);

```


</br>

** LOF 도출 과정 출처
- https://en.wikipedia.org/wiki/Local_outlier_factor (위키피디아)
- https://dodonam.tistory.com/307
- https://godongyoung.github.io/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D/2019/03/11/Local-Outlier-Factor(LOF).html 
- https://www.youtube.com/watch?v=wADcqMdpuv4 (유튜브인데 이게 젤 이해 잘 됨)
