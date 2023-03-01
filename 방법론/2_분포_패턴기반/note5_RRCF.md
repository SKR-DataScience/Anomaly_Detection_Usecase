# 분포기반 이상탐지 - Robust Random Cut Forest

</br>
</br>

> ## 정의
* 트리 기반 이상 탐지 모델의 한 종류로, 데이터의 범위를 반영해 분기하여 실시간으로 이상 탐지를 할 수 있는 모델

  * Isolation Forest와 작동 방식이 유사하나, 두 가지 차이점이 있음

  </br>

  1) 분기할 변수를 선택할 때 uniform random하게 뽑는 대신, 각 feature가 갖는 값의 범위에 따라 확률을 다르게 부여하여 선택함 </br>
      - 주어진 데이터셋 S에 대하여, robust random cut tree: T(S)는 다음과 같이 만들어짐 </br>
        </br>   
        ![image](https://user-images.githubusercontent.com/55543156/221072164-4dd65f44-84c4-4418-ab75-ab8c82fcace5.png)   
        
      - feature가 갖는 값의 범위가 변함에 따라, 이를 반영하여 서로 다른 확률을 부여한 뒤 분기 기준을 다시 선택하는 원리.
      - 이 작은 차이만으로 시간에 따라 분포가 달라지는 데이터에 대응하여 실시간 트리를 만들 수 있음.

      - 새로운 데이터가 들어왔을 때, 샘플에 대해 전체 트리를 다시 만드는게 아니라   
        RRCF만의 노드 추가/제거 알고리즘을 통해 First-In-First-Out 방식으로 새로운 데이터를 계산함. 근데 설명을 봐도 모르겠음... 

  </br>
  
  2) 평균 분기 횟수 대신, Collusive displacement (CoDisp) 라는 새로운 anomaly score를 만들어 이상치를 판단함 </br>
       - 이상 데이터를 제거했을 때 남은 데이터에서 depth가 감소하는 수를 활용하여 점수 계산 (모델의 복잡성 감소 변화)
         </br>
         ![image](https://user-images.githubusercontent.com/55543156/221073270-2616c059-d35f-4d57-a1cb-0cb781c23be8.png) </br>
         --> 파란 범위 데이터로 만든 RRCT에 비해, 노란 점까지 추가해 만든 RRCT의 경우에는   
             노란 점을 분기하기 위해 언젠가 한 번의 분기가 더 필요함 
             (초반에 분기되든 마지막에 분기되든, 전체적으로 한 번은 더 분기되어야 함)   
             --> **이상치의 자매 노드에 있는 데이터들은 이상치가 없을 때에 비해 path length가 1만큼 증가**
       
       </br> 
       
   - 이상 스코어 Disp(x,S) : 데이터셋S로 만든 RRCT에서 데이터x를 제거했을 때, 남은 데이터에서 발생한 depth 변화의 총합
     ![image](https://user-images.githubusercontent.com/55543156/221073972-5445db43-fa73-4277-8d18-416883f16e9c.png) </br>
     - 그림을 보면, x를 제거했을 때 생기는 depth 변화의 총합은 **데이터 x의 자매 노드에 있는 데이터 개수**와 같음   
       (자매노드에 있는 데이터들의 depth가 각각 1씩 감소)

      </br> 
      
   - CoDisp(x,S) 로 발전 : 이상데이터x 근처에 비슷한 데이터(공모자)들이 있다면, 그들은 맨 마지막 근처 노드에서 분기될 것이기 때문에 Disp 값이 매우 작음
                           x를 제거했을 때의 Disp값이 아니라, x와 그의 공모자 집합인 C를 제거했을 때 발생하는 depth 변화 총합을 계산
                           (C는 x를 포함한 조상노드들의 부분집합으로 정의)    
     ![image](https://user-images.githubusercontent.com/55543156/221074690-ac088e75-69f3-4807-846d-6c313d6a5efa.png) </br>

      --> x의 조상노드 부분집합 C를 제거했을 때 자매노드의 데이터 개수를 각각 구한 뒤, 그 중 최대값을 CoDisp 값으로 선정

</br>
</br>

> ## 장단점

[장점]
- 분포가 변화하는 Streaming data에 유리함
- Batch / Streaming 두 상황 모두에서 활용 가능
- Isolation Forest와 마찬가지로 Subsampling을 통해 연산량 감소 가능

[단점]
- 분리를 위한 선을 수직과 수평으로만 자름 -> Extended Isolation Forest 방법을 통해 해결 가능 

</br>
</br>

> ## 현업사례
- 장비 장애 이력을 활용한 사전 이상 탐지

</br>
</br>


> ## 코드




          
 

   
       

