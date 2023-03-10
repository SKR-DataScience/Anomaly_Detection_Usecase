# 개요6. 실제 현업 적용 사례 - 게임
- 게임 Domain에서 이뤄지는 이상 탐지 실제 사례 스터디

</br>

## 1. 게임 머니(Game Money) 이상 탐지

- [출처 링크](https://tack98tistory.com/42?category=454780)

- 문제 정의
  - 게임 내 재화가 계속 늘어나면 인플레이션 현상 발생 -> 화폐 가치 하락, 신규 유저와 기본 유저간 빈부격차 발생 -> 신규 유저 이탈 
  - 게임 내 버그나 어뷰징으로 인해 재화가 급격하게 증가하면 게임 서비스에 치명적인 영향을 줌 (재화 복사 버그가 뒤늦게 발견되면 서버 롤백까지 진행될 수 있음)
  - 따라서, 게임 도메인에서는 초기에 이상 탐지 하는 것이 매우 중요함
  
- 기대효과
  - 게임 운영 정상화
  - 버그 및 어뷰징 유저 사전 탐지를 통한 리스크 감소

- 이상 데이터 정의
  - **시간대 고려**. 새벽 시간대는 유저들의 활동이 적으므로, 주요 시간대의 재화 증감량과 비슷할 수 있음
  - 시간변수 이외 서버점검, 레이드와 같은 이벤트 기반의 변수 고려 필요 (레이드: 단체 팀플해서 퀘스트 깨는 것)
  
- 구체적 해결방법

  [1차 이상탐지] -> rough하게 먼저 처리
    - **정상 데이터 활용** 회귀방정식을 통한 재화 예측 모델 만듦 (Y: 재화증가량, X: 주기/이벤트 등)
    - STL 분해(시계열 데이터를 다양한 요인으로 나누어보는 것)을 활용하여 Trend 데이터 뽑아내고, x 데이터에 추가 반영
    - **신뢰수준을 설정**하고, 예측값에 상한과 하한을 결정 (시계열 모델은 신뢰수준 설정 가능)
    - 상한과 하한을 일정 횟수(Threshold)를 넘어가는 경우에 알람을 보냄
  
  [2차 이상탐지] -> critical한 것 처리
    - 위 1차 탐지에서는 재화가 급격하게 변화하는 경우만 탐지 가능 (상한/하한 넘는 경우)
      -> 교묘하게 서서히 재화를 증가시키는 경우는 1차에서 탐지 어려움 (소수의 어뷰저가 지속적으로 버그를 악용하는 경우)
    - 2차 이상 탐지는 잘못된 재화 증감량이 너무 작아 1차에서 발견되지 않은 값을 대상으로 함 -> Anomaly 재정의
    - **예측 값과 실제 값의 차이인 잔차를 이용**
    - 정상 상태: 잔차는 0을 평균으로 하는 정규 분포 형태로 나타남
    - 이상 현상 발생: 잔차가 한 쪽으로 쏠린 분포로 나타남 -> Threshold 설정 필요
  
- 성과
  - 모델 적용 결과, 이상 사건 탐지 뿐 아니라, 사람들이 과금하는 포인트 및 주기성에 대한 패턴도 발견하게 됨

- 결론
  - 이상탐지에 생각보다 엄청 복잡한 방법을 사용하지는 않음
  - 어려운 방법을 쓰면 쓸수록 현업에 설명하기 어려움
  - 내 데이터와 현업에서 직면한 문제를 고려하여 효과적인 해결 방법을 생각해내는 게 중요 (무조건 딥러닝 쓰기보다는)
