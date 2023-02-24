# 시퀀스 기반 이상탐지 - STL

</br>
</br>

> ## 정의
- 시계열 분해 후 잔차를 활용하여 데이터 이상을 탐지하는 기법

  * 다양한 상황에서 사용할 수 있는 강력한 시계열 분해 기법
  * 월별, 분기별 포함 어떤 종류의 계절성도 다를 수 있음
  * 계절적인 성분이 시간에 따라 변해도, Hyper parameter를 통해 반영 가능
  * Trend와 Seasonality를 제거하고 남은 Residual을 활용해 이상치 탐지

  ![image](https://user-images.githubusercontent.com/55543156/221077277-5cd029a6-e8ed-453d-80cd-330e896770aa.png)
